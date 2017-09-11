# Queries
Of course, what is data service with queries?

**naee** provide the class Query, powerful and easy to use. Query comes packet with many predicates that can leverage all the needs.

## Your first, simple query
Let’s say that you want to fetch all the documents of the collection “Test” that has the title attribute equals to “Rome”.

```
let collection = Collection(name: “Test”)
let query = Query()
	.where(“title”, isEqualTo: “Rome”)
collection.fetchDocuments(query: query) { documents, error in
	
}
```

## Conditioms and query parameters
A Query may be composed by several search conditions, and some additional parameters that define the behavior of the fetch.

### Search conditions

This is the list of all the search predicates provided by Query

#### Comparison conditions 

* where(attribute, isEqualTo:)
* where(attribute, isNotEqualTo:)
* where(attribute, containsString:)
* where(attribute, contains:)
* where(attribute, isGreaterThan:)
* where(attribute, isGreaterOrEqualThan:)
* where(attribute, isLessThan:)
* where(attribute, isLessOrEqualThan:)

#### Geo-location conditions
* where(attribute, isNear:, point)
* where(attribute, isWithin:, points)

#### Composite searches (or/and)
Typically, multiple where predicates create an “and” composite search, i.e. all the conditions are to be true. If you want create some search composition, you can use the “or” and ”and” composite methods:

```
let yesterday = Date().adding(calendarUnit: .day, value: -1)
let query = Query()
	.where(“title”, isEqualTo: “Rome”)
	.or([
		Query.where(“createdAt”, isGreaterThan: date),
		Query.where(“updatedAt”, isGreaterThan: date)
	])
```

Translated in human language:

Find all documents whom title is Rome that are created or updated after yesterday this time.

## Query additional parameters
Upon searching criteria, you can specify additional parameters that can influence the result of your query:

### Sorting
To sort the result, use the `sort` method:

```
let query = Query()
  .sort(by: “title”, ascending: true)
  .sort(by: “updatedAt”, ascending: false)
```

In the above example, fetched documents are first sorted by title, in ascending order, then by update date, in descending order, i.e. starting from last updated document. 

### Limit
This parameter limits the the number of documents retrieved from the query. An example:

```
let query = Query()
	.setLimit(50)
``` 

Limit default value is 10.

### Include
Normally, for collections with relational data, the fetch call returns references of the related documents, not the actual documents. If you include the relation names, the fetch call will include the complete related documents, actually recomposing the data graph.

```
let query = Query()
	.include(“customer”)
let collection = Collection(name: “Invoice”)
collection.fetchDocuments(query: query) { invoices, error in
	if let firstInvoice = invoices.first {
		print(firstInvoice.customer.string(for: “company”)
	]
}
```
If you don’t specify the included relations, related object will be returned in their simpler form, then you’ll have to fetch each one independently. The simplified form of a Document is essentially his id and type, without the rest if its attributes; to check if a document is fully or partially fetched, check for its isVault attribute: if true, the document has only the reference attributes. To resolve a “vault” document, call its fetch method:

```
// document.string(for: “title”) is nil
if document.isVault {
	document.fetch { error in
		if error == nil {
			print(document.string(for: “title”)) // Title is now available
		}
	}
}
```

For more on this, see [Relations](relations.md)

### Select
Select parameter is useful to limit the attributes fetched from the server. Say that you want only to fetch document’s “title“ attribute:

```
let query = Query()
	.select([“title”])
collection.fetchDocuments(query: query) {documents, error in
	if let firstDocument = documents.first {
		print(firstDocument.string(for: “title”)) // available
		print(firstDocument.string(for: “amount”)) // nil
	}
}
```

## Using NSPredicate in Swift SDK
Cocoa developers know very well the NSPredicate class and its flexibility. If you prefer to work with NSPredicate, you can create Query object with ease:

```
let query = Query(withPredicate: myPredicate)
```

