# Documents
Documents are the equivalent of records in common databases. They are stored in collections (see [Collections](collections.md))
The data structure of a document is defined by the Collection schema.
For example the Invoice collection will contain Document objects, each of them containing data related to the Invoice data structure:

## Invoice collection Schema

| Attribute Name | Attribute Type |
|:-|:-|
| number | Text |
| customer | Relation to Customer |
| date | Timestamp |
| amount | Number |
| payed | Boolean |

## Create a new invoice Document
```
let customer = ... // obtained elsewhere
let collection = Collection(name: "Invoice") 
let invoice = Document(collection: collection)
invoice.set(int: 202, for: "number")
invoice.set(document: customer, for: "customer")
invoice.set(date: Date(), for: "date")
invoice.set(double: 39.99, for: "amount")
invoice.set(boolean: false, for: "payed")
invoice.save { error in

	if error == nil {
		// Invoice document created
		print(invoice.id!) 
	}
}
```