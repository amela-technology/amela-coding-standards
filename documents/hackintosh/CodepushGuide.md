# CodePush Guide

1. Cho phép các nhà phát triển React Native triển khai các bản cập nhật ứng dụng dành cho thiết bị di động trực tiếp đến thiết bị của người dùng mà không cần trợ giúp của Store (cập nhật OTA).
2. Hữu ích hơn để sửa lỗi hoặc thêm / xóa các tính năng nhỏ không yêu cầu xây dựng lại và phân phối lại thông qua Store.

## Getting Stated

Bạn có thể cài đặt codepush bằng cách nhập dòng lệnh sau vào trong terminal của bạn:

```shell
npm install --save react-native-code-push
```

Sau đó bạn tiếp tục cài đặt các native module.

1. Android setup
2. IOS setup

## Android Setup

### Plugin Installation and Configuration for React Native 0.60 version and above (Android)

1. Trong file `settings.gradle` trong thư mục `android` bạn hãy thêm dòng sau vào cuối cùng của file:

```gradle
...
include ':app', ':react-native-code-push'
project(':react-native-code-push').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-code-push/android/app')
```

2. Trong file `build.gradle` trong thư mục `android\app` hãy thêm build task definition `codepush.gradle` dưới `react.gradle`

```gradle
...
apply from: "../../node_modules/react-native/react.gradle"
apply from: "../../node_modules/react-native-code-push/android/codepush.gradle"
...
```

3. Cập nhật file `MainApplication.java` để sử dụng CodePush theo hướng dẫn dưới đây:

```java
...
// 1. Import the plugin class.
import com.microsoft.codepush.react.CodePush;

public class MainApplication extends Application implements ReactApplication {

    private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
        ...

        // 2. Override the getJSBundleFile method in order to let
        // the CodePush runtime determine where to get the JS
        // bundle location from on each app start
        @Override
        protected String getJSBundleFile() {
            return CodePush.getJSBundleFile();
        }
    };
}
```

4. Thêm Deployment key vào `strings.xml`:

```xml
 <resources>
     <string name="app_name">AppName</string>
     <string moduleConfig="true" name="CodePushDeploymentKey">DeploymentKey</string>
 </resources>
```

#### Plugin Installation (Android - RNPM)

1.  Bạn hãy chạy dòng lệnh sau trong terminal nếu bạn sử dùng React Native phiên bản v0.27

```
react-native link react-native-code-push
```

Nếu bạn sử dụng phiên bản thấp hơn v0.27 hãy chạy dòng lệnh sau:

```
rnpm link react-native-code-push
```

2. Nếu bạn đang sử dụng RNPM >=1.6.0, bạn sẽ được gợi ý cho deployment key. Nếu bạn chưa có, bạn có thể nhận được giá trị này bằng cách chạy dòng lệnh sau `appcenter codepush deployment list -a <ownerName>/<appName> -k`, hoặc bạn có thể chọn cách ignore nó (bằng cách làm ẩn `<ENTER>`) và thêm nó vào sau. Để bắt đầu, chúng tôi recommend bạn sử dụng deployment key `Staging`, do đó bạn có thể test CodePush end-to-end.

#### Plugin Installation (Android - Manual)

1. Trong file `android/settings.gradle` hãy thêm theo như hướng dẫn:

```gradle
include ':app', ':react-native-code-push'
project(':react-native-code-push').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-code-push/android/app')
```

2. Trong file `android/app/build.gradle` thêm `:react-native-code-push` vào trong dependencies như sau:

```gradle
...
dependencies {
    ...
    compile project(':react-native-code-push')
}
```

3. Trong file `android/app/build.gradle` Thêm `codepush.gradle` như là additional build task definition dưới `react.gradle`:

```gradle
...
apply from: "../../node_modules/react-native/react.gradle"
apply from: "../../node_modules/react-native-code-push/android/codepush.gradle"
...
```

### Plugin Configuration for React Native lower than 0.60 (Android)

#### For React Native v0.29 - v0.59

##### For newly created React Native application

Nếu bạn đang tích hợp Code Push vào ứng dụng React Native mới, vui lòng thực hiện các bước sau:

Cập nhật file `MainApplication.java` để dùng CodePush theo những thay đổi dưới đây:

```java
...
// 1. Import the plugin class.
import com.microsoft.codepush.react.CodePush;

public class MainApplication extends Application implements ReactApplication {

  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
      ...
      // 2. Override the getJSBundleFile method in order to let
      // the CodePush runtime determine where to get the JS
      // bundle location from on each app start
      @Override
      protected String getJSBundleFile() {
          return CodePush.getJSBundleFile();
      }

      @Override
      protected List<ReactPackage> getPackages() {
          // 3. Instantiate an instance of the CodePush runtime and add it to the list of
          // existing packages, specifying the right deployment key. If you don't already
          // have it, you can run "appcenter codepush deployment list -a <ownerName>/<appName> -k" to retrieve your key.
          return Arrays.<ReactPackage>asList(
              new MainReactPackage(),
              new CodePush("deployment-key-here", MainApplication.this, BuildConfig.DEBUG)
          );
      }
  };
}
```

\_NOTE: Đối với React Native v0.49+ hãy chắc chắn rẳng hàm `getJSMainModuleName` trong file `MainApplication.java` định nghĩa đúng URL để fetch JS bundle (được sử dụng khi tính năng hỗ trợ nhà phát triển được bật, xem ở [đây](https://github.com/facebook/react-native/blob/c7f37074ac89f7e568ca26a6bad3bdb02812c39f/ReactAndroid/src/main/java/com/facebook/react/ReactNativeHost.java#L124) để biết thêm thông tin chi tiết) e.g.\*

```
@Override
protected String getJSMainModuleName() {
    return "index";
}
```

##### For existing native application

Nếu bạn muốn tích hợp CodePush vào 1 ứng dụng React Native đã tồn tại thì hãy làm theo hướng dẫn dưới đây:

Cập nhật file `MyReactActivity.java` (Tên của nó có thể sẽ khác trong project của bạn) để sử dụng CodePush hãy làm theo những thay đổi sau:

```java
...
// 1. Import the plugin class.
import com.microsoft.codepush.react.CodePush;

public class MyReactActivity extends Activity {
    private ReactRootView mReactRootView;
    private ReactInstanceManager mReactInstanceManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ...
        mReactInstanceManager = ReactInstanceManager.builder()
                // ...
                // Add CodePush package
                .addPackage(new CodePush("deployment-key-here", getApplicationContext(), BuildConfig.DEBUG))
                // Get the JS Bundle File via Code Push
                .setJSBundleFile(CodePush.getJSBundleFile())
                // ...

                .build();
        mReactRootView.startReactApplication(mReactInstanceManager, "MyReactNativeApp", null);

        setContentView(mReactRootView);
    }

    ...
}
```

#### For React Native v0.19 - v0.28

Cập nhật file `MainActivity.java` để sử dụng CodePush hãy làm theo những hướng dẫn dưới đây:

```java
...
// 1. Import the plugin class (if you used RNPM to install the plugin, this
// should already be done for you automatically so you can skip this step).
import com.microsoft.codepush.react.CodePush;

public class MainActivity extends ReactActivity {
    // 2. Override the getJSBundleFile method in order to let
    // the CodePush runtime determine where to get the JS
    // bundle location from on each app start
    @Override
    protected String getJSBundleFile() {
        return CodePush.getJSBundleFile();
    }

    @Override
    protected List<ReactPackage> getPackages() {
        // 3. Instantiate an instance of the CodePush runtime and add it to the list of
        // existing packages, specifying the right deployment key. If you don't already
        // have it, you can run "appcenter codepush deployment list -a <ownerName>/<appName> -k" to retrieve your key.
        return Arrays.<ReactPackage>asList(
            new MainReactPackage(),
            new CodePush("deployment-key-here", this, BuildConfig.DEBUG)
        );
    }

    ...
}
```

#### Background React Instances

_Phần này rất cần thiết nếu bạn sử dụng <b>explicitly</b> để launching 1 ứng dụng React Native instance mà không dùng `Activity`. Trong những tình huống như thế này, CodePush phải được hướng dẫn cách tìm phiên bản React Native của bạn._

Để update/restart phiên bản React Native của bạn, CodePush phải được configured với một `ReactInstanceHolder` trước khi cố gắng khởi động lại một phiên bản trong nền. Nó sẽ chạy trong implementation `Application` của bạn.

##### For React Native >= v0.29 (Background React Instances)

Cập nhật file `MainApplication.java` để sử dụng CodePush hãy làm theo những thay đổi dưới đây:

```java
...
// 1. Declare your ReactNativeHost to extend ReactInstanceHolder. ReactInstanceHolder is a subset of ReactNativeHost, so no additional implementation is needed.
import com.microsoft.codepush.react.ReactInstanceHolder;

public class MyReactNativeHost extends ReactNativeHost implements ReactInstanceHolder {
  // ... usual overrides
}

// 2. Provide your ReactNativeHost to CodePush.

public class MainApplication extends Application implements ReactApplication {

   private final MyReactNativeHost mReactNativeHost = new MyReactNativeHost(this);

   @Override
   public void onCreate() {
     CodePush.setReactInstanceHolder(mReactNativeHost);
     super.onCreate();
  }
}
```

##### For React Native v0.19 - v0.28 (Background React Instances)

Trước v0.29, React Native đã không cung cấp 1 abstraction `ReactNativeHost`. Nếu bạn chạy 1 phiên bản trong nền, bạn sẽ phải tự build `ReactInstanceHolder`. Ví dụ:

```java
// 1. Provide your ReactInstanceHolder to CodePush.

public class MainApplication extends Application {

   @Override
   public void onCreate() {
     // ... initialize your instance holder
     CodePush.setReactInstanceHolder(myInstanceHolder);
     super.onCreate();
  }
}
```

Để hiệu quả hãy sử dụng `Staging` và `Production` deployments, cái mà được tạo cùng với ứng dụng CodePush của bạn, đọc doc này [multi-deployment testing](../README.md#multi-deployment-testing) trước khi thực sự sử dụng CodePush vào ứng dụng product.

#### WIX React Native Navigation applications

Nếu ứng dụng của bạn sử dụng [WIX React Native Navigation **version 1.x**](https://github.com/wix/react-native-navigation), hãy làm theo những bước sau đây để tích hợp CodePush:

1. Không cần thay đổi file `MainActivity.java`, Nếu bạn tích hợp CodePush vào một ứng dụng RNN được tạo mới file đó sẽ chỉ như này:

```java
import com.facebook.react.ReactActivity;
import com.reactnativenavigation.controllers.SplashActivity;

public class MainActivity extends SplashActivity {

}
```

2. Hãy cập nhật file `MainApplication.java` để sử dụng CodePush theo những thay đổi dưới đây:

```java
// ...
import com.facebook.react.ReactInstanceManager;

// Add CodePush imports
import com.microsoft.codepush.react.ReactInstanceHolder;
import com.microsoft.codepush.react.CodePush;

public class MainApplication extends NavigationApplication implements ReactInstanceHolder {

	@Override
	public boolean isDebug() {
		// Make sure you are using BuildConfig from your own application
		return BuildConfig.DEBUG;
	}

	protected List<ReactPackage> getPackages() {
		// Add additional packages you require here
		return Arrays.<ReactPackage>asList(
			new CodePush("deployment-key-here", getApplicationContext(), BuildConfig.DEBUG)
		);
	}

	@Override
	public List<ReactPackage> createAdditionalReactPackages() {
		return getPackages();
	}

	@Override
	public String getJSBundleFile() {
        // Override default getJSBundleFile method with the one CodePush is providing
		return CodePush.getJSBundleFile();
	}

	@Override
	public String getJSMainModuleName() {
		return "index";
	}

    @Override
    public ReactInstanceManager getReactInstanceManager() {
        // CodePush must be told how to find React Native instance
        return getReactNativeHost().getReactInstanceManager();
    }
}
```

Nếu ứng dụng của bạn dựa trên [WIX React Native Navigation **version 2.x**](https://github.com/wix/react-native-navigation/tree/v2) hãy làm theo các bước dưới đây để có thể tích hợp CodePush

1. Giống như trong tài liệu React Native Navigation's, `MainActivity.java` nên extend `NavigationActivity`:

```java
import com.reactnativenavigation.NavigationActivity;

public class MainActivity extends NavigationActivity {

}
```

2. Cập nhật file `MainApplication.java` để sử dụng CodePush theo những thay đổi sau:

```java
// ...
import com.facebook.react.ReactInstanceManager;

// Add CodePush imports
import com.microsoft.codepush.react.CodePush;

public class MainApplication extends NavigationApplication {

    @Override
    public boolean isDebug() {
        return BuildConfig.DEBUG;
    }

    @Override
    protected ReactGateway createReactGateway() {
        ReactNativeHost host = new NavigationReactNativeHost(this, isDebug(), createAdditionalReactPackages()) {
            @javax.annotation.Nullable
            @Override
            protected String getJSBundleFile() {
                return CodePush.getJSBundleFile();
            }

        };
        return new ReactGateway(this, isDebug(), host);
    }

    @Override
    public List<ReactPackage> createAdditionalReactPackages() {
        return Arrays.<ReactPackage>asList(
	    new CodePush("deployment-key-here", getApplicationContext(), isDebug())
	    //,MainReactPackage , etc...
    }
}
```

### Code Signing setup

Bắt đầu với phiên bản CLI **2.1.0** bạn có thể tự sign các bundles trong quá trình phát hành và xác minh signature của nó trước khi cài đặt bản cập nhật. Để biết thêm thông tin về Code Signing, vui lòng tham khảo[relevant code-push documentation section](https://github.com/microsoft/code-push/tree/v3.0.1/cli#code-signing). Để sử dụng Public key cho việc Code Signing, bạn cần thực hiện các bước sau:

Thêm `CodePushPublicKey` vào `/path_to_your_app/android/app/src/main/res/values/strings.xml`. Nó sẽ trông như thế này:

```xml
<resources>
   <string name="app_name">my_app</string>
   <string name="CodePushPublicKey">-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtPSR9lkGzZ4FR0lxF+ZA
P6jJ8+Xi5L601BPN4QESoRVSrJM08roOCVrs4qoYqYJy3Of2cQWvNBEh8ti3FhHu
tiuLFpNdfzM4DjAw0Ti5hOTfTixqVBXTJPYpSjDh7K6tUvp9MV0l5q/Ps3se1vud
M1/X6g54lIX/QoEXTdMgR+SKXvlUIC13T7GkDHT6Z4RlwxkWkOmf2tGguRcEBL6j
ww7w/3g0kWILz7nNPtXyDhIB9WLH7MKSJWdVCZm+cAqabUfpCFo7sHiyHLnUxcVY
OTw3sz9ceaci7z2r8SZdsfjyjiDJrq69eWtvKVUpredy9HtyALtNuLjDITahdh8A
zwIDAQAB
-----END PUBLIC KEY-----</string>
</resources>
```

#### For React Native <= v0.60 you should configure the `CodePush` instance to use this parameter using one of the following approaches

##### Using constructor

```java
new CodePush(
    "deployment-key",
    getApplicationContext(),
    BuildConfig.DEBUG,
    R.string.CodePushPublicKey)
```

##### Using builder

```java
new CodePushBuilder("deployment-key-here",getApplicationContext())
   .setIsDebugMode(BuildConfig.DEBUG)
   .setPublicKeyResourceDescriptor(R.string.CodePushPublicKey)
   .build()
```

## IOS Setup

​

### Plugin Installation and Configuration for React Native 0.60 version and above (iOS)

1. Chạy `cd ios && pod install && cd ..` để install các CocoaPods dependencies cần thiết.
   ​
2. Mở file `AppDelegate.m` và sau đó import statement cho CodePush:

   ```objective-c
   #import <CodePush/CodePush.h>
   ```

3. Tìm những dòng code chứa URL để phân phối sản phẩm:

   ```objective-c
   return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
   ```

4. Thay thế nó bằng dòng code này

   ```objective-c
   return [CodePush bundleURL];
   ```

5. Thêm Deployment key vào `Info.plist`:

   Để cho phép CodePush tự biết thời gian nào nó cần cập nhật, mở file `Info.plist` và thêm 1 cái key `CodePushDeploymentKey`, có giá trị là key của việc triển khai mà bạn muốn cấu hình ứng dụng này (Giống như key cho `Staging` deployment cho app `FooBar` ). Bạn có thể nhận giá trị bằng cách chạy dòng lệnh sau `appcenter codepush deployment list -a <ownerName>/<appName> -k` trong AppCenter CLI ( `-k` cần thiết khi các keys không xuất hiện default) và cần copy đúng `Key` vào cái development bạn muốn dùng. Chú ý trong việc sử dụng deployment's name (ví dụ như là Staging) sẽ không hoạt đông.

   ![Deployment list](https://cloud.githubusercontent.com/assets/116461/11601733/13011d5e-9a8a-11e5-9ce2-b100498ffb34.png)

   Để hiệu quả hãy sử dụng `Staging` và `Production` deployments được tạo đồng thời trong CodePush app, đọc doc này [multi-deployment testing](../README.md#multi-deployment-testing) trước khi bạn thực sự dùng CodePush cho production.

### Plugin Installation for React Native < 0.60 (iOS)

Để đáp ứng nhiều tùy chọn của nhà phát triển nhất có thể, plugin CodePush hỗ trợ cài đặt iOS thông qua ba cơ chế:

1. [**RNPM**](#plugin-installation-ios---rnpm) - [React Native Package Manager (RNPM)](https://github.com/rnpm/rnpm) là một công cụ tuyệt vời cung cấp trải nghiệm cài đặt đơn giản nhất có thể cho các plugin React Native. Nếu bạn đã sử dụng nó hoặc bạn muốn sử dụng nó, thì chúng tôi khuyên bạn nên sử dụng phương pháp này.

2. [**CocoaPods**](#plugin-installation-ios---cocoapods) - Nếu bạn đang xây dựng một ứng dụng iOS gốc đang nhúng React Native vào đó hoặc đơn giản là bạn thích sử dụng [CocoaPods](https://cocoapods.org), thì chúng tôi khuyên bạn nên sử dụng Podspec
3. [**"Manual"**](#plugin-installation-ios---manual) - Nếu bạn không muốn phụ thuộc vào bất kỳ công cụ bổ sung nào hoặc vẫn ổn với một vài bước cài đặt bổ sung thì hãy thực hiện theo phương pháp này.

#### Plugin Installation (iOS - RNPM)

1. Kể từ v0.27 của React Native, `rnpm link` đã được hợp nhất vào React Native CLI. Đơn giản chỉ cần chạy:

   ```
   react-native link react-native-code-push
   ```

   Nếu ứng dụng của bạn sử dụng phiên bản React Native thấp hơn v0.27, hãy chạy như sau:

   ```
   rnpm link react-native-code-push
   ```

   _Note: Nếu bạn chưa cài đặt RNPM, bạn có thể làm như vậy bằng cách chỉ cần chạy `npm i -g rnpm` và sau đó thực hiện lệnh trên. Nếu bạn đã cài đặt RNPM, hãy đảm bảo rằng bạn có v1.9.0 + để được hưởng lợi từ cài đặt một bước này._

2. Bạn sẽ được nhắc nhập key triển khai mà bạn muốn sử dụng. Nếu bạn chưa có, bạn có thể truy xuất giá trị này bằng cách chạy `appcenter codepush deployment list -a <ownerName>/<appName> -k`, hoặc bạn có thể chọn bỏ qua nó (bằng cách đơn giản ẩn `<ENTER>`) và thêm nó vào sau. Để bắt đầu, chúng tôi khuyên bạn chỉ nên sử dụng khóa triển khai `Staging` của mình, để bạn có thể thử nghiệm CodePush end-to-end.

#### Plugin Installation (iOS - CocoaPods)

1. Thêm các phụ thuộc plugin ReactNative và CodePush vào `Podfile` của bạn, chỉ vào đường dẫn nơi NPM đã cài đặt nó

   ```
   # React Native requirements
   pod 'React', :path => '../node_modules/react-native', :subspecs => [
      'Core',
      'CxxBridge', # Include this for RN >= 0.47
      'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
      'RCTText',
      'RCTNetwork',
      'RCTWebSocket', # Needed for debugging
      'RCTAnimation', # Needed for FlatList and animations running on native UI thread
      # Add any other subspecs you want to use in your project
   ]
   # Explicitly include Yoga if you are using RN >= 0.42.0
   pod 'yoga', :path => '../node_modules/react-native/ReactCommon/yoga'
   pod 'DoubleConversion', :podspec => '../node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
   pod 'glog', :podspec => '../node_modules/react-native/third-party-podspecs/glog.podspec'
   pod 'Folly', :podspec => '../node_modules/react-native/third-party-podspecs/Folly.podspec'

   # CodePush plugin dependency
   pod 'CodePush', :path => '../node_modules/react-native-code-push'
   ```

   _NOTE: Đường dẫn trên cần phải liên quan đến `Podfile` của ứng dụng của bạn, vì vậy hãy điều chỉnh nó nếu cần._

   _NOTE: phiên bản thư viện `JWT` >= version 3.0.x_

2. Chạy `pod install`

_NOTE: CodePush `.podspec` phụ thuộc vào `React` pod, Và vì vậy để đảm bảo rằng nó có thể sử dụng chính xác phiên bản React Native mà ứng dụng của bạn được xây dựng, hãy đảm bảo xác định sự phụ thuộc của `React` trong `Podfile` của ứng dụng của bạn như đã giải thích[here](https://facebook.github.io/react-native/docs/integration-with-existing-apps.html#podfile)._

#### Plugin Installation (iOS - Manual)

1. Mở app's Xcode project của bạn

2. Tìm file`CodePush.xcodeproj` trong đường dẫn `node_modules/react-native-code-push/ios` (hoặc `node_modules/react-native-code-push` <=`1.7.3-beta` installations) avà kéo thả nó vào trong `Libraries` node của Xcode

   ![Add CodePush to project](https://cloud.githubusercontent.com/assets/8598682/13368613/c5c21422-dca0-11e5-8594-c0ec5bde9d81.png)

3. Chọn project node trong Xcode và chọn tab "Build Phases" trong cấu hình dự án của bạn.

4. Kéo `libCodePush.a` từ `Libraries/CodePush.xcodeproj/Products` vào phần "Link Binary With Libraries" trong cấu hình "Build Phases" của dự án của bạn.

   ![Link CodePush during build](https://cloud.githubusercontent.com/assets/516559/10322221/a75ea066-6c31-11e5-9d88-ff6f6a4d6968.png)

5. Nhấp vào dấu cộng bên dưới danh sách "Link Binary With Libraries" và chọn thư viện `libz.tbd` bên dưới node ` iOS 9.1`.

   ![Libz reference](https://cloud.githubusercontent.com/assets/116461/11605042/6f786e64-9aaa-11e5-8ca7-14b852f808b1.png)

   _Note: Bạn có thể thêm `-lz` vào trường `Other Linker Flags` trong `Linking` của `Build Settings`._

### Plugin Configuration for React Native lower than 0.60 (iOS)

_NOTE: Nếu bạn đã sử dụng RNPM hoặc 'react-native link` để tự động liên kết plugin, các bước này đã được thực hiện cho bạn nên bạn có thể bỏ qua phần này. _

Sau khi dự án Xcode của bạn đã được thiết lập để xây dựng / liên kết plugin CodePush, bạn cần định cấu hình ứng dụng của mình để hỏi CodePush về vị trí của gói JS của bạn, vì nó có trách nhiệm đồng bộ hóa nó với các bản cập nhật được phát hành tới máy chủ CodePush. Để thực hiện việc này, hãy thực hiện các bước sau:

1. Mở file `AppDelegate.m` và import statement CodePush:

   ```objective-c
   #import <CodePush/CodePush.h>
   ```

For React Native 0.59 - 0.59.10:

2. Tìm dòng mã sau để tải Gói JS của bạn từ tệp nhị phân của ứng dụng cho các bản phát hành chính thức:

   ```objective-c
   return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
   ```

3. Thay thế nó bằng dòng này:

   ```objective-c
   return [CodePush bundleURL];
   ```

Dành cho React Native <=0.58:

2. Tìm dòng mã sau để tải Gói JS của bạn từ tệp nhị phân của ứng dụng cho các bản phát hành chính thức:

   ```objective-c
   jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
   ```

3. Thay thế nó bằng dòng này:

   ```objective-c
   jsCodeLocation = [CodePush bundleURL];
   ```

Thay đổi này giúp ứng dụng của bạn luôn tải phiên bản mới nhất của gói JS của ứng dụng. Trong lần khởi chạy đầu tiên, điều này sẽ tương ứng với tệp được biên dịch với ứng dụng. Tuy nhiên, sau khi bản cập nhật được đẩy qua CodePush, điều này sẽ trả về vị trí của bản cập nhật được cài đặt gần đây nhất.

_NOTE: Phương thức `bundleURL` giả sử gói JS của ứng dụng của bạn có tên là` main.jsbundle`. Nếu bạn đã định cấu hình ứng dụng của mình để sử dụng một tên tệp khác, chỉ cần gọi phương thức `bundleURLForResource:` (giả sử bạn đang sử dụng phần mở rộng `.jsbundle`) hoặc`bundleURLForResource: withExtension:`để thay thế, để ghi đè lên hành vi mặc định _

Thông thường, bạn sẽ chỉ muốn sử dụng CodePush để giải quyết vị trí gói JS của mình trong các bản phát hành và do đó, chúng tôi khuyên bạn nên sử dụng macro bộ xử lý trước `DEBUG` để tự động chuyển đổi giữa việc sử dụng máy chủ packager và CodePush, tùy thuộc vào việc bạn có đang gỡ lỗi hay không. Điều này sẽ giúp đơn giản hơn nhiều để đảm bảo bạn có được hành động phù hợp mà bạn muốn trong quá trình sản xuất, trong khi vẫn có thể sử dụng Công cụ dành cho nhà phát triển của Chrome, tải lại trực tiếp, v.v. tại thời điểm gỡ lỗi.

Dành cho React Native 0.59 - 0.59.10:

```objective-c
- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge
{
  #if DEBUG
    return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
  #else
    return [CodePush bundleURL];
  #endif
}
```

Dành cho React Native 0.49 - 0.58:

```objective-c
NSURL *jsCodeLocation;

#ifdef DEBUG
    jsCodeLocation = [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
#else
    jsCodeLocation = [CodePush bundleURL];
#endif
```

Dành cho React Native =<0.48:

```objective-c
NSURL *jsCodeLocation;

#ifdef DEBUG
    jsCodeLocation = [NSURL URLWithString:@"http://localhost:8081/index.ios.bundle?platform=ios&dev=true"];
#else
    jsCodeLocation = [CodePush bundleURL];
#endif
```

![Deployment list](https://cloud.githubusercontent.com/assets/116461/11601733/13011d5e-9a8a-11e5-9ce2-b100498ffb34.png)

Để hiệu quả hãy sử dụng `Staging` và `Production` deployments, cái mà được tạo cùng với ứng dụng CodePush của bạn, đọc doc này [multi-deployment testing](../README.md#multi-deployment-testing) trước khi thực sự sử dụng CodePush vào ứng dụng product.

### HTTP exception domains configuration (iOS)

Plugin CodePush thực hiện các yêu cầu HTTPS đối với các miền sau:

- codepush.appcenter.ms
- codepush.blob.core.windows.net
- codepushupdates.azureedge.net

Nếu bạn muốn thay đổi cấu hình bảo mật HTTP mặc định cho bất kỳ miền nào trong số các miền này, bạn phải xác định [`NSAppTransportSecurity` (ATS)][ats] configuration bên trong file **Info.plist** của bạn:

```xml
<plist version="1.0">
  <dict>
    <!-- ...other configs... -->

    <key>NSAppTransportSecurity</key>
    <dict>
      <key>NSExceptionDomains</key>
      <dict>
        <key>codepush.appcenter.ms</key>
        <dict><!-- read the ATS Apple Docs for available options --></dict>
      </dict>
    </dict>

    <!-- ...other configs... -->
  </dict>
</plist>
```

Trước khi làm, xin hãy đoc nó [read the docs][ats].

[ats]: https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW33

### Code Signing setup

Bắt đầu với phiên bản CLI **2.1.0** bạn có thể tự sign các bundles trong quá trình phát hành và xác minh signature của nó trước khi cài đặt bản cập nhật. Để biết thêm thông tin về Code Signing, vui lòng tham khảo[relevant code-push documentation section](https://github.com/microsoft/code-push/tree/v3.0.1/cli#code-signing).

Để configure Public Key cho việc xác thực bundle bạn cần thêm record trong `Info.plist` với cái tên `CodePushPublicKey` và 1 giá trị string nội dung key. Ví dụ:

```xml
<plist version="1.0">
  <dict>
    <!-- ...other configs... -->

    <key>CodePushPublicKey</key>
        <string>-----BEGIN PUBLIC KEY-----
MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBANkWYydPuyOumR/sn2agNBVDnzyRpM16NAUpYPGxNgjSEp0etkDNgzzdzyvyl+OsAGBYF3jCxYOXozum+uV5hQECAwEAAQ==
-----END PUBLIC KEY-----</string>

    <!-- ...other configs... -->
  </dict>
</plist>
```
