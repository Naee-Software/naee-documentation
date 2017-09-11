# Real Time
CuBoid has a powerful Real Time system. Provides immediate notifications for a lot of events, from document changes, through queue updates, even custom notifications.

## Documents notifications
Each time a document is created, updated or deleted, the backend sends to your app a notification. All you have to do is start listening for one or more of the events. You can even provide a query and be notify only for changes on the specific conditions.

```
let collection = Collection(name: “Customer”)
collection.startListening(for: [.created, .updated]) { eventType, documents in

		// documents are created, updated or deleted
}
```

## Queue notifications
When new messages (QueueMessage) are pushed on a Queue, you can receive the proper notification. As for documents, the required action si to start listening for events for the specified queue.

```
let queue = Queue(name: “Chat”)
queue.startListening(for: [.created]) {eventType, messages in

		// Some new messages pushed to the Queue
}
```

## Custom notifications
Other than documents and queues, you can send custom real time notifications on arbitrary channels, on which your user can subscribe to.

To send custom notifications:
```
let notification = Notification(on: “channel”, data: “Hello”)
notification.send {error in
}
```

To subscribe and be notified:
```
Notification.startListen(on: “channel”) { data in
		// data is “Hello”
}
```