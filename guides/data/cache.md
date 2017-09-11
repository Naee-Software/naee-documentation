# Offline caching

## Cache policy

```
enum CachePolicy {
    case never
    case standard
    case once
    case timed(interval: TimeInterval)
    case hourly
    case daily
    case weekly
}
```

*CachePolicy* can be applied to:

- Client, to provide a default caching behavior
- Collection object to provide a Collection’s default caching behavior 
- During fetch in order to apply the policy only to that fetch
- For File documents the policy is applied from the Client and from the fetch in witch the files are retrieved. For File’s data the policy is applied from the Client.

## Implementation details

#### Location and naming
##### Location 
`~\caches\*clientId*\*shared*|*private*`

- *clientId* is client Project id 
- *shared* folder contains cache than can be shared between devices or bundled into app
- *private* folder contains cache that will remain in the current device only

*note: the shared and private folder has not to reside under the same parent folder*

##### File name

**collection_queryhash.fetch**
Contains a fetch result for the given collection and provided query. If no query is provided the name is the only name of the collection. 

**fileId.file**
Contains a single file by its fileId and file name
