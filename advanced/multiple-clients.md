# Multiple Clients
For common situations one Project has direct correspondence with an app, i.e. only one app communicate with its backend.

In these cases, all you have to do in your App is configure the provided, default Client instance: `Client.default`. All the subsequent SDK usage will assume the use of the default Client, therefore the parameter “client” can be omitted. For example, the same code is equivalent:

```
let collection = Collection(name: “Test”, client: Client.default)
let collection = Collection(name: “Test”)
```

In some advanced configurations, your app may need to communicate with more that one project, or simply needs different Client setups through its code implementation. For example:

- When your App needs to access more than one **naee** Project
- When your App needs to configure more than one

For these needs, you can create your Client instances and decide in your code wich client to use. 

To instantiate a new Client, different from the default provided:

```
let myClient1 = Client(configuration: ...client configuration)
let myClient2 = Client(configuration: ...client configuration)
```

Then, in subsequent SDK usage:

```
let collectionFromClient1 = Collection(name: “Test”, client: myClient1)
let collectionFromClient2 = Collection(name: “Demo”, client: myClient2)
```

## Why I have to create more clients for the same Project?
Since each Client uses an underlying URLSession, as long as specific configuration. Creating more than one Client, allows your to keep independent such configurations.

Here some of the specific Client setups:

- URLSession
	- request timeout
	- session behavior during background tasks
- Socket client, used for real time services
- Stage mode, logging and debug settings
- Document mappings (see [Document subclassing](document-subclassing.md))