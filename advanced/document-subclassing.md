# Document Subclassing
The Document class gives you access to all the functionality to save, delete and update its content. For example, to set an hypothetical title, just insert:

```
myDocument.set(string: “Hello”, for: “title”)
```

That’s easy, fast and good for most of the usage. For complex models, this solution is sub-optimal:

* Hardcoded attribute names are error prone and difficult to refactor
* There is no mechanism for value observation
* You may need computed value, on reading and before writing

For these reasons, would be nice to subclass Document into more specialized model entities. **naee** SDKs for Swift and Android allow you to do that.

## Step 1: Subclass Document to your model entity
The first step is create a Document subclass that reflect your model entity, for example:

```
class Invoice: Document {

	var amount: DecimalNumber {
		get {
			return decimalNumber(for: “amount”) ?? DecimalNumber.zero
		}
		set {
			set(decimalNumber: newValue, for: “amount”)
		}
	}
		
	var payedAmount: DecimalNumber {
		get {
			return decimalNumber(for: “payed”) ?? DecimalNumber.zero
		}
		set {
			set(decimalNumber: newValue, for: “payed”)
		}
	}
	
	var isPayed: Bool {
		return payedAmount == amount
	}
}
```

In the above example, the specialized Invoice class has “direct” properties for the amount and the payedAmount, as long a convenience property to check if the Invoice is payed.

## Step 2: Define a Document Mappings

Subclassing a Document is enough when creating a document. The problem is that when you fetch documents from a Collection, the Client doesn’t know that you want, from the example above, Invoice objects and not simple Document.

To instruct your Client to instantiate the desired object types, simply provide a DocumentMappings array in the client’s configuration phase. For example: 

```
let documentMappings = [
	“Invoice”: Invoice.self
]
let configuration = Client.Configuration(
	clientId: “your client id”,
	clientKey: “your client key”,
	documentMappings: documentMappings
)
Client.default.configure(configuration)
```

In the above configuration we are saying to the Client that every time is trying do create a Document from the “Invoice” collection, it has to give us an Invoice object instead.

That’s so powerful, isn’t it?

## Advanced and more granular use
Other than the Client’s configuration, you can specify the document class in the collection’s scope or for single fetches:

```
let invoiceCollection = Collection(name: “Invoice”, documentClass: Invoice.self)
... elsewhere in your code
invoiceCollection.fetchDocuments { invoices, error in
	...
}
```

Or

```
let collection = Collection(name: “Invoice”)
collection.fetchDocuments(documentClass: Invoice.self) { invoices, error in
	...
}
```

This advanced usage is good if, for some reasons, a specific query needs to return specific Document subclasses.