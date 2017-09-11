# Business Modules
In addition to the standard services, **naee** deploys some vertical modules, able to interact with third part services; these modules belong to the **nare** concept called Service-in-Service (SiS). With SiS, apps and websites can leverage the powerfulness of specific services or platforms using the same paradigms and components used in the whole **naee** SDKs.
For example, using the **Pay** Module, implementing a CC payment transaction is easily done in few line of code, without any knowledge of the payment system used and, moreover, without the need to implement the server-side code to manage the payment.

## Store
This module contains components and services for managing eCommerce related items, such as Catalogs, Carts and Orders.
The module is able to interact and sync with the major eShop platforms:
* Shopify
* PrestaShop (coming soon)
* wCommerce (coming soon)
Other integration will be developed in the future.
### Main entities
|Entity|Description|
|:--|:--|
|Product|Contains all the products as **naee** Document object. Each product has the usual property, such as description, price, tags and so on|
|Showcase|Each Showcase is a collection of Products. Showcase can be composed by a list of referenced Products or by a query of Products|
|Order|Contains the list of items to be purchased. Orders can be  defines also as wish lists and carts|
### Quick examples
#### Fetching products
```
let query = Query(collection: Product.Collection())
    .where("price", isGreaterThan: 100)
query.fetchDocuments { products, error in
	if error == nil {
		self.displayProducts(products)
	}
}
```
#### Create an order 
```
let order = Order(type: .Order)
let item1 = OrderItem(withProduct: product1, quantity: 2)
let item2 = OrderItem(withProduct: product2, quantity: 1)
order.addItems([item1, item2])
order.send { error in
	if error == nil {
		self.completeCheckout()
	}
}
```

## Pay
Payment systems are hard to implement for some reasons:
1. Security and privacy are to be taken with care
2. Client code is not enough; you need to implement a middle-tier in your server, in order to communicate between the client and the payment system

**naee** Pay handle all this with two simple classes (Payment and Transaction) and a secure server-side service, able to interact with several payment systems, such as Stripe and PayPal.
### Entities
|Class|Description|
|:--|:--|
|Payment|Defines the specific payment that will be used for a subsequent transaction. Can be CC details or a Customer reference|
|Transaction|A transaction is a specific payment or refund operation. Can be a standalone transaction or tied with a related Order, made with the Store module|
### Quick examples
#### Make a standalone payment
```
let payment = Payment(type: .cc, card: "*******", expirationMonth: "01", expirationYear: "2020", cvv: "***")
let transaction = Transaction(payment: payment,
															title: "Yearly subscription",
															currency: "EUR",
															amount: 99.99)
transaction.send { error in
	if error == nil {
		self.showConfirmation()
	}
}
```
#### Make a Store payment
```
let order = Order(type: .Order)
let item1 = OrderItem(withProduct: product1, quantity: 2)
let item2 = OrderItem(withProduct: product2, quantity: 1)
order.addItems([item1, item2])
order.payment = Payment(type: .cc, card: "*******", expirationMonth: "01", expirationYear: "2020", cvv: "***")
order.send { error in
	if let transaction = order.transaction {
		if transaction.success == true {
			self.completeCheckout()
		} else {
			self.displayTransactionError(transaction)
		}
	}
}
```
## Geo
Geo module provides some useful services related to geographical content. 

- Specialized Placemark collection, a container for geographical related venues
- Integration with Foursquare, Google Maps and other geo providers
- Geofencing and triggering 
## Feeds
It’s a module for handling information content, such as newsfeeds and source providers

- A specialized Feed collection, for content storage
- Integration with feed sources, such as Wordpress, RSS, ATOM