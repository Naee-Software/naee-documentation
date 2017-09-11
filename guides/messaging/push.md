# Push Notifications
Sending Push Notifications from your app is straightforward with **naee**:
1. Configure your push settings for the Project
2. On app launch or user login, register the device
3. Create a PushNotification object
4. Instruct the PushManager to send your message, optionally providing destination criteria

```
let message = PushMessage(“Hello pros!”)
PushManager.send(message) {
    if error = nil {
        // message was correctly sent to all registered device
    }
}
```
## Configure your Project for Push Notifications
In **naee Studio**, for each push system that your app will support, provide the credential or certificates.
### Apple
You have to provide at least one Push Certificate, obtained from the Apple Developer Portal.
#### Multi Push certificates
Unlike the most of BaaS around, **naee** Push modulo can handle more than one Apple Push certificate for the same project. Why that? Mainly because of how Apple limits the usage of Push Certificate: each one is linked with one and only one bundle identifier. Say that you have a project that has to be used in two different app (a pro and a client version); if the client version has to send push notifications to the pro edition, you need to configure two different push systems, each one with the appropriate Push Certificate. With **naee** that is not necessary; all you have to do is configure two push profiles and use the preferred one while sending the notification.

```
let message = PushMessage(“Hello pros!”)
PushManager.send(message, using: “pro”) {
    if error = nil {
        // message was correctly sent to all pro apps
    }
}
```

### Android
Configure the push settings by providing the API key for your app, created in Android developer portal. 
## Register the user’s device
In order to receive Push Notifications, each user’s device has to be registered. This will create a new Device document in the Device collection. Each Device contains the identifier (or token) provided by the os and other informations, such as the os used, the timezone and customizable metadata. 
Tipically your app register the device during the launch operations. You can choose to postpone the registration of the device in a later stage, for example after the user login.
### On iOS/macOS 
For Apple systems you have to enable push notifications, the handle the app delegate `didRegisterRemoteNotification` method:

```
// Enable push notification

...
// In your app delegate
func didRegisterRemoteNotification(token: Data, error: NSError) {

    Device.registerDevice(withToken: token) { error in
        if error == nil {
            // device ready to receive notifications
        }
    }
}
```
