# Access Control List (ACL)
Handling the access to most of the **naee** entities is made through the ACL object, a simple structure that defines how access has to be controller.

Entities that can be controlled by ACL are:

* Documents
	* Users
	* Roles
	* Devices
* Files
* Queues
* Queue Messages

In order to control access rights of one of the above entities, all you have to do is create an ACL objet and associate it to the single entity. Here an example:

```swift
let user = ...an user...
let document = ...a document...
...
let acl = ACL()
acl.grantReadAccess(to: [user])
acl.revokeWriteAccess(to: [user])
document.acl = acl
document.save...
```

## Manipulating the ACL
### Read and Write access
Every ACL object has two main access right:
#### Read access
The users or roles associated with this right can read the entity 
#### Write access
The users or roles associated with this right can write (update) the entity
### Public access
The default ACL object for every entity grants public read and write access. This can be made in code with two properties:

```swift
acl.publicReadAccess = true
acl.publicWriteAccess = true
```
Public access does not exclude specific access rights defined in the following section. What this means is that, for example, you disable public Read access, you can still give read access to some users. On the opposite, you enable public read access, you can still deny access to some users.
### Granting and denying access to users or roles
If read/write access is disabled you have to specify which users or roles have access to the ACL controlled entity:
#### Granting access
Gives to some users or roles the right to read or write the associated entities.

```swift
acl.grantReadAccess(to: [user1, user2, role])
```

*Note*: Remember that this override the public access behavior.
#### Denying access
Deny (or revoke) some users or roles to read or write the entities controller by an ACL object. 

```swift
acl.denyReadAccess(to: [user1, user2, role])
```

*Note*: Remember that this override the public access behavior.

## Obtaining ACL informations
Setting ACL is crucial, but sometime is necessary to inspect an ACL to know the ACL state. Some methods are provided:

* `readAccessUsers`: the list of users than can read the entity associated with the ACL
* `writeAccessUsers`: the list of users that can update the entity associated with the ACL
* `readAccessRoles`: the list of roles than can read the entity associated with the ACL
* `writeAccessRoles`: the list of roles that can update the entity associated with the ACL
* `isUserAllowedToRead(user)`: check if the user has read access right
* `isUserAllowedToWrite(user)`. check if the user has write access right

### Querying ACL on documents
Query can be used also to search for certain ACL rights:

```
let collection = Collection(name: “Test”)
let query = Query(collection: collection)
  .where(publicReadAccess: false)
	.where(allowedReadRoles: [role1, role2])
query.fetchDocuments { documents, error in
	// Documents with read access on role1 and role2
}
```

## Tips and recommendations 

* Remember to set the ACL do the entity (Document or other ACL settable object) after you edited the ACL itself, i.e. `document.acl = myACL`
* Remember to save the entity after setting its ACL, i.e. `document.save...`
* Be aware that tehoretically you can deny the right to read a newly created document even to the current logged user, excluding the user itself to access the document he created!
* Tend to grant or deny accesso to Roles. Grant or deny access to Users only in case when they’re few, and normally when the public access is disabled. A good is example is to give read access to documents accessible only to a small group of users (see following examples).

## Examples
Grant read access only to a small group of users:

```swift
let users = ...few users...
acl.publicReadAccess = false
acl.grantReadAccess(to: users)
```

Grant read access to everybody but limit write access to few:

```swift
let users = ...few users...
acl.publicReadAccess = true
acl.publicWriteAccess = false
acl.grantWriteAccess(to: users)
```

Grant read access to everyone except to users of the “Guest” Role:

```swift
Role.fetch(id: roleId) { role, error in
	if let role = role {
		let acl = ACL()
		acl.publicReadAccess = true
		acl.denyReadAccess(to: role)	
		myDocument.acl = acl
		myDocument.save { error in 
			if error == nil {
				// document and ACL saved
			}
		}
	}
}
```