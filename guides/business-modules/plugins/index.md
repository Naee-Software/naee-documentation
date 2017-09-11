# Plugins

Plugins are JavaScrips procedures executed on your behalf from the Backend. There are two main categories of plugins:

* Custom plugins
* Triggers

You write plugins in CuBoid Studio, into the Plugin Manager. 

## Custom plugins
Arbitrary methods executed on demand, from a client’s requested.
Triggers have a custom name and a variable set of parameters. You call them with the Client’s execute method:
```
Client.default.execute(plugin: “recalcCart”,
										  parameters: {“userId”: id}) { response, error in
										  
}
```

## Triggers
Methods automatically executed when some events happens to remote documents. They are called automatically on the following conditions:

* Before a document’s creation or update (beforeUpdate)
* After a document’s creation or update (afterUpdate)
* Before a document’s deletion (beforeDelete)
* After a document’s deletion (afterDelete)
* Before a document is fetched by a query (onFetch)

Every trigger plugin provide the document involved in the operation and, with the exception of delete plugins, receive the transformed version of the document.

Change the price if a document is of type 2
```
beforeUpdate (document) {
	if (document.type == 2) {
			document.price *= 2.0;
	}
	return document;
}
```

Delete corresponding transaction when a document is deleted
```
afterDelete (document) {
	var query = Query().isEqual(“id”, document.transaction.id);
	var collection = Collection(“Transaction”)
	collection.find(query, function(transaction) {
			transaction.delete();
	});
}
```