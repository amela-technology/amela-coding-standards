## Giới thiệu

Khi phát triển app, developer bắt buộc phải sử dụng biến môi trường:

- Môi trường DEV: Phát triển ứng dụng trên local
- Môi trường STAGING (Tuỳ dự án): Build ứng dụng trước khi đẩy lên production
- Môi trường PRODUCTION: Build cho end user Để cài đặt biến môi trường hãy sử
  dụng thư viện react-native-config. Link:
  <https://github.com/luggit/react-native-config>

## Cài đặt cho iOS

## Phân môi trường + Thêm vào file .env

- Trong Xcode. Mở Product > Scheme > Edit Scheme
- Click Duplicate Scheme ở bottom
- Đổi tên theo cấu trúc: “Tên dự án” + “Môi trường (Production, Staging)”
- Set chế độ “Shared” để có thể push lên git.
  ![1](https://user-images.githubusercontent.com/30967592/102464426-e95a5380-407e-11eb-839b-29a40c1b6ef9.png)

- Sau khi tạo xong hãy trỏ đường dẫn file .env tham chiếu với môi trường vừa tạo
  Pre-actions => New Run Script Action `echo ".env.production" > /tmp/envfile #`
  replace .env.production for your current environment
  ![2](https://user-images.githubusercontent.com/30967592/102464444-ecedda80-407e-11eb-8ebe-115c36684c20.png)

- Thêm key vào file env
  ![3](https://user-images.githubusercontent.com/30967592/102464448-ee1f0780-407e-11eb-92d4-40a8b8c56bb7.png)

## Thiết lập cho Build settings

- Bấm vào cây thư mục và tạo 1 file mới dạng XCConfig
  ![4](https://user-images.githubusercontent.com/30967592/102464450-ee1f0780-407e-11eb-8657-65d274b1ee26.png)

  ![5](https://user-images.githubusercontent.com/30967592/102464454-ef503480-407e-11eb-9d91-f81010f67298.png)

- Lưu file đó dưới tên Config.xcconfig với nội dung như sau:
  `#include? "tmp.xcconfig"`
- Thêm dòng sau vào file .gitignore:

`react-native-config codegen` `ios/tmp.xcconfig`

- Đi tới Project settings và apply file Config.xcconfig mình vừa tạo
  ![6](https://user-images.githubusercontent.com/30967592/102464457-ef503480-407e-11eb-814d-d6d1466c5066.png)

- Tạo 1 Build phase cho scheme mình vừa tạo. Thêm dòng bên dưới vào sau phần
  “echo … > tmp/envfile”
  `"${SRCROOT}/../node_modules/react-native-config/ios/ReactNativeConfig/BuildXCConfig.rb" "${SRCROOT}/.." "${SRCROOT}/tmp.xcconfig"`
  ![7](https://user-images.githubusercontent.com/30967592/102464476-f5deac00-407e-11eb-9e86-d865184907b5.png)

## Run project sau đó chuyển sang bước tiếp theo

- Đi tới project của bạn ⇒ Build Settings ⇒ All
  ![8](https://user-images.githubusercontent.com/30967592/102464480-f70fd900-407e-11eb-8584-97283604717c.png)

- Tìm kiếm với từ khóa preprocess Đổi `Preprocess Info.plist File` thành `Yes`
  Đổi `Info.plist Preprocessor Prefix File` thành
  `${BUILD_DIR}/GeneratedInfoPlistDotEnv.h` Đổi
  `Info.plist Other Preprocessor Flags` thành `-traditional` Nếu không thấy thì
  vui lòng check lại xem “All” đã được chọn ở trên cùng nhé (Thường được hay
  chọn là “Basic”)
  ![9](https://user-images.githubusercontent.com/30967592/102464483-f7a86f80-407e-11eb-8f34-65c421eb33bf.png)

## Thiết lập cho Info.plist

- Đi tới file Info.plist Đổi `CFBundleDisplayName` thành `RNC_APP_NAME` Đổi
  `CFBundleShortVersionString` thành `RNC_IOS_APP_VERSION_CODE` Đổi
  `CFBundleVersion` thành `RNC_IOS_APP_BUILD_CODE`
  ![10](https://user-images.githubusercontent.com/30967592/102464497-ff681400-407e-11eb-9eff-ae87867c996f.png)

## Cài đặt cho Android

- Trước hết bạn nên có 1 file .keystore (vì môi trường production sẽ cần đến cái
  này). Lệnh để gen ra file keystore:
  `keytool -genkeypair -v -keystore DEMO-key.keystore -alias DEMO-alias -keyalg RSA -keysize 2048 -validity 10000`
  (Nhớ thay thế DEMO thành tên keystore bạn muốn nhé. Sau đó đặt vào folder
  android/app)
  ![11](https://user-images.githubusercontent.com/30967592/102464501-00994100-407f-11eb-9611-9aec9dacfae4.png)

- Mở thư mục android, mở file gradle.properties lên và thêm dòng này vào cuối
  file đó: `MYAPP_UPLOAD_STORE_FILE=DEMO-key.keystore`
  `MYAPP_UPLOAD_KEY_ALIAS=DEMO-alias`
  `MYAPP_UPLOAD_STORE_PASSWORD=<<password của bạn>>`
  `MYAPP_UPLOAD_KEY_PASSWORD=<<password của bạn>>` Mình nghĩ nên để password là
  amela@123 để cho dễ nhớ.
  ![12](https://user-images.githubusercontent.com/30967592/102464502-0131d780-407f-11eb-9cea-89bc4c657a45.png)

- Mở thư mục android, mở folder app, mở file build.gradle Copy đoạn này lên trên
  cùng của file build.gradle

      `project.ext.envConfigFiles = [`
      `dev: ".env.development",`
      `staging: ".env.staging",`
      `product: ".env.production",`
      `anothercustombuild: ".env",`
      `]`

  ![13](https://user-images.githubusercontent.com/30967592/102464506-01ca6e00-407f-11eb-98b8-aa835c7e0019.png)

- Tìm đến dòng có `apply from: "../../node_modules/react-native/react.gradle"`,
  copy dòng sau xuống ngay dưới
  `apply from: project(':react-native-config').projectDir.getPath() + "/dotenv.gradle"`
  ![14](https://user-images.githubusercontent.com/30967592/102464509-01ca6e00-407f-11eb-9e88-0674486e7857.png)

- Tìm đến `android.defaultConfig`, ví dụ project Honeything như sau:
  ![15](https://user-images.githubusercontent.com/30967592/102464512-02630480-407f-11eb-9fad-4bb9cea66ad6.png)

  Thay thế giá trị `“com.honeything”` của `applicationId` thành
  `env.get("ANDROID_APP_ID")` Thay thế giá trị `1` của `versionCode` thành
  `Integer.valueOf(env.get("ANDROID_APP_VERSION_CODE"))` Thay thế giá trị
  `“1.0”` của `versionName` thành `env.get("ANDROID_APP_VERSION_NAME")` Thêm
  dòng sau vào cuối của object defaultConfig
  `resValue "string", "build_config_package", "<<packageName>>”` (Chú ý là
  <<packageName>> sẽ tìm ở trong android/app/src/main/AndroidManifest.xml )
  ![16](https://user-images.githubusercontent.com/30967592/102464516-02fb9b00-407f-11eb-9df7-bf744c8c0eec.png)

- Sau tất cả, chúng ta có:
  ![17](https://user-images.githubusercontent.com/30967592/102464521-02fb9b00-407f-11eb-90db-11836b7df45e.png)

- Tìm đến android.signingConfig, thêm đoạn sau vào ngay dưới object debug

      ```
      release {
      if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) {
      storeFile file(MYAPP_UPLOAD_STORE_FILE)
      storePassword MYAPP_UPLOAD_STORE_PASSWORD
      keyAlias MYAPP_UPLOAD_KEY_ALIAS
      keyPassword MYAPP_UPLOAD_KEY_PASSWORD
      }
      }
      ```

  ![18](https://user-images.githubusercontent.com/30967592/102464525-03943180-407f-11eb-9573-bc51d319f8fc.png)

- Bên dưới android.buildTypes, copy đoạn sau vào:

      ```
      flavorDimensions "enviroment"
      productFlavors {
      dev {
      dimension "enviroment"
      resValue "string", "app_name", project.env.get("APP_NAME")
      signingConfig signingConfigs.debug
      }
      staging {
      dimension "enviroment"
      resValue "string", "app_name", project.env.get("APP_NAME")
      signingConfig signingConfigs.debug
      }
      product {
      dimension "enviroment"
      resValue "string", "app_name", project.env.get("APP_NAME")
      signingConfig signingConfigs.release
      }
      }
      ```

  ![19](https://user-images.githubusercontent.com/30967592/102464526-042cc800-407f-11eb-8c94-3ccdc2c6c4f6.png)

- Vào file `android/app/src/main/res/values/strings.xml` để xóa hết các thẻ
  <string/> đi, chỉ để lại mỗi <resource/> để tránh bị duplicate project name
