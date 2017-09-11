# Sync and Async methods

For best flexibility, Naee Swift SDK provides Sync and Async versions of every method that requires remote calls.

It is important to note that, regardless of the method chosen, all the remote calls are made in background threads, through the URLSession tasks, therefore the difference is only between the usage that you’ll consider to make.
## Async
Those methods use Swift blocks to return the results of the remote call, such the objects retrieved and, eventually, the error encountered. The method returns immediately and doesn’t block the calling thread. This is the preferred kind of method.
## Sync
With this topology of method, the calling thread is blocked until the result is returned. Sync methods throw error via the standard Swift error handling, therefore they have to be prefixed with the usual try statement.

**Don’t call sync methods from the main thread of your app. If you have to, surround the calls by an async dispatch**

Tipical usage of sync methods:

* Sequenced/ordered API calls (see examples)
* Usage in server environment such as Vapor

### Sequenced/ordered API calls
```
DispatchQueue.global().async {
		let collection = Collection(name: “MyCollection”)
		let query = Query(collection: collection)
		do {
				let document = try query.fetchFirstDocument()
				document.set(string: “new title”, for: “title”)
				try document.save()
				DispatchQueue.main.async {
						// Update your UI
				}
		} catch {
				// We have an error
		}
}
```

### Vapor usage
```
Func index(_ req: Request) -> throws {

		// Get the id from the request
		guard let id = req.parameters[“id”]?.string else {
			throw NError(code: 400, message: “User not found”) 
		}
		
		// Get the document from Naee
		let collection = Collection(name: “MyCollection”)
		let query = Query(collection: collection)
		let document = try query.fetchDocument(withId: id)
		
		// Show the leaf page with document content
		return make(“document”, document, req)
}
```
In this example note that Document is conforming to Vapor’s Node, as implemented in the Naee Vapor SDK.

### Client.perform

Client class provides a method that executing in a background thread a sequence of SyncMethods, eventually catching errors in a provided error handler:

```
Client.default.perform({
    let collection = Collection(name: "Test")
    let forms = try.fetchDocuments()
}) { error in
    // Some error happened
}
```

## Choosing the right method
As a rule of thumb, use the Async methods and process the response in the method handler.

Go for the Sync methods when:

* You have to control the flow of multiple calls (the alternative way would be to use Async methods with DispatchGroups or DispatchSemaphores)
* You are working in a server environment, such as Vapor, where the calling thread is managed for you
* You are calling from background threads and find sync methods more readable