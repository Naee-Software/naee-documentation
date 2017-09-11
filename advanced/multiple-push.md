# Multiple Push profiles

Unlike the most of BaaS around, **naee** Push modulo can handle more than one Apple Push certificate for the same project. 

## Why that? 
Mainly because of how Apple limits the usage of Push Certificate: each one is linked with one and only one bundle identifier. 

Say that you have a project that has to be used in two different app (a pro and a client version); if the client version has to send push notifications to the pro edition, you need to configure two different push systems, each one with the appropriate Push Certificate. With **naee** that is not necessary; all you have to do is configure two push profiles and use the preferred one while sending the notification.

## Apple Push configurations 
In **naee Studio** you can configure has many configuration you need for Apple Push. Each configuration has:

- An identifier, used to choose the proper configuration during send
- A Certificate, obtained from the Apple Developer Portal
- An option that states if the configuration is a production or developer one (more on that later)

## How To specify which configuration to use
When sending a push message, you can specify which profile to use. In the following example our app will send the message to all devices with the Pro Version of the app, the one identified with the profile “pro”.

```
let message = PushMessage(“Hello pros!”)
PushManager.send(message, using: “pro”) {
    if error = nil {
        // message was correctly sent to all pro apps
    }
}
```

Of course the same option is available in the other SDKs, in order to reach Apple devices.