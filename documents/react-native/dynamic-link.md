Tài liệu cài đặt của react-native-firebase: [https://rnfirebase.io/dynamic-links/usage](https://rnfirebase.io/dynamic-links/usage) 

Tài liệu đọc hiểu của Google: [https://firebase.google.com/docs/dynamic-links](https://firebase.google.com/docs/dynamic-links)

### Add an app to project on Firebase
Config for IOS :

IOS 

- Bundle ID
- Apple Store ID ( Nếu không có Apple ID thì có thể sử dụng 1 ID bất kỳ (9 số) với bản DEV)
- TeamID

### Chú ý :

- Associated Domain ở tab Signing & Capabilities trong Xcode phải được điền đúng.
- Trong Tab Info trong Xcode trường Identifier ( Điền là text "Bundle ID" ) ,trường URL Schemes(Điền Bundle ID của app)

### Config for Android :

ANDROID

- packageName
- SHA-1/SHA-256

```jsx
// addd this line to AndroidManifest.xml
	android:launchMode="singleTask"
```

## Dynamic links là gì ?

Firebase Dymic Links là những liên kết, cái mà làm việc theo ý của bạn, trên nhiều nền tảng.

Giả sử có một người dùng open một dynamic link thì sẽ được chỉ dẫn đến nội dung được liên kết trong ứng dụng hoặc website đã được cài đặt theo ý của bạn.

Nếu trên thiết bị của bạn chưa cài đặt ứng dụng thì bạn sẽ được yêu cầu cài đặt. Sau khi cài đặt thì ứng dụng của bạn sẽ bắt đầu và truy cập vào link.

 

### How does it work ?

Bạn tạo một DL bằng cách sử dụng Firebase console, sử dụng một REST API,  IOS hoặc Android Builder API, hoặc bằng cách điền thông tin một URL  bằng cách thêm tham số DL tới một domain specific cho ứng dụng của bạn. Những cái tham số này chỉ định liên kết bạn muốn mở, phụ thuộc vào những nền tảng người dùng và việc ứng dụng của bạn đã được cài đặt hay chưa. 

 

### Custom link domains ?

Cách thứ nhất, tạo DL bằng tên domain của bạn: [https://firebase.google.com/docs/dynamic-links/custom-domains](https://firebase.google.com/docs/dynamic-links/custom-domains)

Cách thứ hai, sử dụng domain phụ có dạng [page.link](http://page.link). Để tạo subdomain miễn phí có thể làm theo hướng dẫn trên Firebase console. 

Tất cả các tính năng của DL, bao gồm phân tích, tích hợp SDK , hoạt động với cả tên miền [page.link](http://page.link)  và domain của bạn.

 

### Implementation path?

1. Setup Firebase and the Dynamic Links SDK 
2. Create Dynamic Links 
3. Handle Dynamic Links in your app
4. View analytics data

 

### Operating System Integrations

### Create link in code by react-native-firebase :

Link : [https://rnfirebase.io/reference/dynamic-links/dynamiclinkparameters](https://rnfirebase.io/reference/dynamic-links/dynamiclinkparameters)

```jsx
const link = await dynamicLinks().buildShortLink({
						//link để mở web khi click trên browser
            link: `${Config.URL_WEB}/song.php?data=${id}&isYoutube=${isYoutube}`,
            domainUriPrefix: Config.DYNAMIC_LINK,
            analytics: {
                campaign: 'banner',
            },
            ios: {
                bundleId: 'jp.ufret.app',
                appStoreId: '1483940246',// cần thiết 
                minimumVersion: '1',
            },
            android: {
                packageName: `${Config.ANDROID_APP_ID}`,
                minimumVersion: '1',
            },
						// trường social để hiển thị khi share
            social: {
                title: title,
                imageUrl: `${Config.URL_WEB}/img/unnamed.png`,
            },
        })
```

### Listening for Dynamic Links

- Foreground events

```jsx
import dynamicLinks from '@react-native-firebase/dynamic-links';

function App() {
  const handleDynamicLink = link => {
    // Handle dynamic link inside your own application
    if (link.url === 'https://invertase.io/offer') {
      // ...navigate to your offers screen
    }
  };

  useEffect(() => {
    const unsubscribe = dynamicLinks().onLink(handleDynamicLink);
    // When the is component unmounted, remove the listener
    return () => unsubscribe();
  }, []);

  return null;
}
```

- Background/Quit events

```jsx
import dynamicLinks from '@react-native-firebase/dynamic-links';

function App() {
  useEffect(() => {
    dynamicLinks()
      .getInitialLink()
      .then(link => {
        if (link.url === 'https://invertase.io/offer') {
          // ...set initial route as offers screen
        }
      });
  }, []);

  return null;
}
```