# Projects
Each **naee** Project is made of several components. Their meaning and semantics are very important in order to better understand how **naee** works. 

A Project is a container where all data, logics and settings lies. 

## Collections
Each Project contains one or more data Collections. When you create a new Project, some collections are automatically created for your, such as User, Role and Device. Some additional modules create specific collections. 
## Schema and Attributes
A schema represents the data structure of each Collection. An Attribute is a semantic element that represent a subdivision of your data. 
As an example, you can define a Collection called Product, with the following schema:

| name | type |
|:-|:-|
| sku | text |
| title | text |
| price | number |
| availability | boolean |
| store | relation to Store |

*as you can see the store Attribute is a relation to a Store collection. More on relations after.*
## Documents
Each Collection contains as many documents your Project needs. Each document is subdivided into values given by the structure of the collection defined by its attributes. 
## Data structure and conventional database
You can see a Project data structure somehow similar to a relational database, where:

| Naee naming | Database semantics |
|:-|:-|
| Project | Database |
| Collection | Table |
| Document | Row |
| Attribute | Column |

## Cache 
Cache is a conveniente and fast way to store data stored in your Project but not in a relational nor structured way. 
Cache are stored as key-value couples, such as localized elements, games assets, and so on. 
## Queues
Queues are list of elements managed by the system in a queued way, such as LIFO (Last In First Out) or FIFO (First In First Out). They can have limited life and delayed in time. 
Queues are useful in order to manage messages or list of task for your users.
See [Queues](../guides/data/queues.md)
## Push Notifications
Part of the Messaging module, provide a standardized way to send push notification to every platform able to support them.
See [Messaging](../guides/messaging/index.md)
## Mail
Also part of the Messaging module, provide a easy way to send mail right from your app.
See [Messaging](../guides/messaging/index.md). 
## Notifications
Part of the Real Time module, enable your app to listen to remote notification, in real time.
Several types of notifications are available:
### Custom notifications
Send and receive message or structured data in custom channels of your choice.
### Document notifications
System notifications automatically triggered every time a Document is created, updated or deleted.
### Queue notifications
System notifications automatically triggered every time a message is added to a Queue
### Other project's components