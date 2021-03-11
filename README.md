# Zowie Flutter SDK

## Requirements
Flutter: 1.20.0+

Android: 21+

iOS: 11+

## Dependency
Add this to your package's pubspec.yaml file:
```dart
dependencies:
  zowie_flutter_sdk: 0.0.1+4
```

## Installation
From command line:
pub:
```dart
pub get
```
Flutter:
```dart
flutter pub get
```

Alternatively, get flutter packages from your editor

## Import
```dart
import 'package:zowie_flutter_sdk/zowie.dart';
import 'package:zowie_flutter_sdk/zowie_color.dart';
```

## iOS Podfile
Edit Podfile at the end, set BUILD_LIBRARY_FOR_DISTRIBUTION as 'YES' for every configuration, as below:
```
post_install do |installer|
  installer.pods_project.targets.each do |target|
    # Zowie SDK
    target.build_configurations.each do |config|
      config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
    end
    # end
    flutter_additional_ios_build_settings(target)
  end
end
```

## Usage
All functions below return Future<bool>
- if true, everything is ok
- if false, something went wrong, check logs



### Configuration
To be able to use Zowie SDK first of all you have to provide the configuration:

```dart
Zowie.configuration(
  userId: "REQUIRED_STRING",
  conversationId: "REQUIRED_STRING",
  instanceId: "REQUIRED_STRING",
  conversationInitReferral: "OPTIONAL_STRING",
  jwt: "OPTIONAL_STRING",
);
```

if `jwt` property is not declared or empty, SDK will use <b>raw</b> authentication

### Show chat
```dart
Zowie.showChat();
```
for <b>Android</b>:

Chat will appear as new activity

for <b>iOS</b>:

Default Flutter iOS project does not contain `UINavigationController`, chat will appear as modal.
To show full screen chat combined with application navigation, wrap default `FlutterViewController` into `UINavigationController` in `AppDelegate`

```swift
import UIKit
import Flutter

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
 override func application(
  _ application: UIApplication,
  didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
 ) -> Bool {
    GeneratedPluginRegistrant.register(with: self)
    
    // Zowie SDK: wrap FlutterViewController into UINavigationController to navigate to chat
    // otherwise chat will appear as modal
    let flutterViewController = window?.rootViewController as! FlutterViewController
    let navigationController = UINavigationController(rootViewController: flutterViewController)
    navigationController.delegate = self // AppDelegate extension below
    navigationController.isNavigationBarHidden = true
    window?.rootViewController = navigationController
    window?.makeKeyAndVisible()
    // end
    
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
  }

}

// Zowie SDK
extension AppDelegate: UINavigationControllerDelegate {
  func navigationController(_ navigationController: UINavigationController, willShow viewController: UIViewController, animated: Bool) {
    guard let navigationController = window?.rootViewController as? UINavigationController else {
      return
    }
    if viewController is FlutterViewController {
      navigationController.setNavigationBarHidden(true, animated: animated)
    } else {
      navigationController.setNavigationBarHidden(false, animated: animated)
    }
  }
}
// end
```

### Metadata
You can set needed user metadata. All those fields are optional:
```dart
Zowie.metadata(
  firstName: "OPTIONAL_STRING",
  lastName: "OPTIONAL_STRING",
  name: "OPTIONAL_STRING",
  locale: "OPTIONAL_STRING",
  timeZone: "OPTIONAL_STRING",
  phoneNumber: "OPTIONAL_STRING",
  email: "OPTIONAL_STRING",
  extraParams: <String, String>{
    "OPTIONAL_STRING": "VALUE"
  }
);
```

### Enable notifications
To receive FCM notifications you have to provide `deviceToken`:
```dart
Zowie.enableNotifications("REQUIRED_STRING");
```

### Disable notifications
If you want to disable notifications:
```dart
Zowie.disableNotifications();
```

### Set user status
To set user's status:
```dart
Zowie.setUserStatus(REQUIRED_BOOL);
```

### Set context
You can feed backend with the `contextId`:
```dart
Zowie.setContext("REQUIRED_STRING");
```

### Localizations
The only supported language in this SDK is `english`. If you need more localization please provide it as below:
```dart
Zowie.localization(
  newMessageHint: "OPTIONAL_STRING",
  messageStatusDelivered: "OPTIONAL_STRING",
  messageStatusSendingErrorMessage: "OPTIONAL_STRING",
  messageStatusSendingErrorTryAgain: "OPTIONAL_STRING",
  readAndWriteStoragePermissionAlertTitle: "OPTIONAL_STRING",
  readAndWriteStoragePermissionAlertMessage: "OPTIONAL_STRING",
  readAndWriteStoragePermissionAlertPositiveButton: "OPTIONAL_STRING",
  readAndWriteStoragePermissionAlertNegativeButton: "OPTIONAL_STRING",
  attachmentPlaceholderName: "OPTIONAL_STRING",
  attachmentFileMaxSizeExceededErrorMessage: "OPTIONAL_STRING",
  couldNotOpenFileErrorMessage: "OPTIONAL_STRING",
  chatConnectionErrorMessage: "OPTIONAL_STRING",
  chatConnectionRestoredMessage: "OPTIONAL_STRING",
  chatHistoryDownloadErrorMessage: "OPTIONAL_STRING",
  couldNotOpenWebBrowserErrorMessage: "OPTIONAL_STRING",
  unexpectedErrorMessage: "OPTIONAL_STRING",
  fileDownloadErrorMessage: "OPTIONAL_STRING",
);
```

### Colors
Feel free to set up color branding however you like:
```dart 
Zowie.colors(
    messageStatusSendingErrorColor:: OPTIONAL_COLOR,
    messageStatusDeliveredColor: OPTIONAL_COLOR,
    sentMessageBackgroundColor: OPTIONAL_ZOWIE_COLOR,
    sentMessageContentsColor: OPTIONAL_COLOR,
    sentMessageImageUploadLoadingColor: OPTIONAL_COLOR,
    sentMessageVideoUploadLoadingColor: OPTIONAL_COLOR,
    sentMessageImagePlaceholderLoadingColor: OPTIONAL_COLOR,
    sentMessageImagePlaceholderBackgroundColor: OPTIONAL_COLOR,
    sentMessageLinksColor: OPTIONAL_COLOR,
    incomingMessageBackgroundColor: OPTIONAL_ZOWIE_COLOR,
    incomingMessagePrimaryTextColor: OPTIONAL_COLOR,
    incomingMessageSecondaryTextColor: OPTIONAL_COLOR,
    incomingMessageFileIconColor: OPTIONAL_ZOWIE_COLOR,
    incomingMessageFileDownloadSuccessIconColor: OPTIONAL_COLOR,
    incomingMessageDownloadFileIconColor: OPTIONAL_COLOR,
    incomingMessageDownloadFileLoadingColor: OPTIONAL_COLOR,
    incomingMessageImagePlaceholderLoadingColor: OPTIONAL_COLOR,
    incomingMessageImagePlaceholderBackgroundColor: OPTIONAL_COLOR,
    incomingMessageLinksColor: OPTIONAL_COLOR,
    backgroundColor: OPTIONAL_COLOR,
    newMessageTextColor: OPTIONAL_COLOR,
    newMessageHintTextColor: OPTIONAL_COLOR,
    sendAttachmentButtonColor: OPTIONAL_COLOR,
    sendTextButtonColor: OPTIONAL_ZOWIE_COLOR,
    separatorColor: OPTIONAL_COLOR,
    chatMessagesLoadingColor: OPTIONAL_COLOR,
    quickButtonBackgroundColor: OPTIONAL_COLOR,
    quickButtonTextColor: OPTIONAL_COLOR,
    quickButtonPressedStrokeColor: OPTIONAL_COLOR,
    actionButtonBackgroundColor: OPTIONAL_COLOR,
    actionButtonBackgroundPressedColor: OPTIONAL_COLOR,
    actionButtonTextColor: OPTIONAL_COLOR,
    videoThumbnailPlaceholderColor: OPTIONAL_COLOR,
    notificationErrorContentsColor: OPTIONAL_COLOR,
    notificationErrorBackgroundColor: OPTIONAL_COLOR,
    notificationSuccessContentsColor: OPTIONAL_COLOR,
    notificationSuccessBackgroundColor: OPTIONAL_COLOR,
    zowieLogoButtonBackgroundPressedColor: OPTIONAL_COLOR,
    zowieLogoButtonPressedStrokeColor: OPTIONAL_COLOR,
    playVideoButtonBackgroundColor: OPTIONAL_COLOR,
    playVideoButtonBackgroundPressedColor: OPTIONAL_COLOR,
    playVideoButtonPlayIconColor: OPTIONAL_COLOR,
    actionButtonShadow: OPTIONAL_COLOR,
);
```
