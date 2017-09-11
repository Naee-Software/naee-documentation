# Custom Notifications
Custom notifications are the simplest for if real time provided by **naee**. You can publish arbitrary data to a channel and subscribe to channels. 
## Channels
Channels are spaces used for real time publishing. When you publish a notification, you have to specify the channel it belongs to. When listening for notifications, you have to specify the channel you’re interested on. 
## Publish a notification
A notification is a piece of data that you want to publish in a given channel. 

```
NotificationManager.default.publish(to: “channel”, data: “A message”) { error in
  if error == nil {
    // message successfully sent
  }
}
```

## Subscribe to a channel
In order to receive messages published on a channel, you have to subscribe to it:

```
NotificationManager.default.startListening(on: “channel”) { data, error in

  if let message = data as? String {
    // new message received
  }
}
```

## End subscription to a channel
To stop receiving notifications from a channel:

```
NotificationManager.default.stopListening(on: “channel”) { error in
  if error == nil {
    // no more notifications 
  }
}

