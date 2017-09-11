# Authentication
## Users
Your app's users are stored in the system User collection. Since it is a regular **naee** Collection, you can extend its schema, in order to include user's attributes specific for your project, such as personal data.
### Authenticating users 
You have two authentication methods:

- Internal authentication, using username and password previously defined during the user's registration
- External authentication, via an authentication provider, such a social network or enterprise system

Note that all users are stored in the User collection, regardless of the authentication method used. Note also that an external authenticated user can be converted in internal, just by providing username and password.

## Roles
Roles are group of users. Each User can belong to one or more roles.

To add a User to a Role:

```
user.addToRole(role)
```

To remove a User from a Role:

```
User.removeFromRole(role)
```