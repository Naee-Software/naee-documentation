# Files
A File is a specialized **naee** Document that you can use to store all the information of the actual file content.
Since remote files can reside in different places, note that a File is technically a reference to the remote file, therefore it doesn't contain data. If your provide a local data content, when you save (create) the File, data is upload to you project's container and the File is created. If you provide an external file reference, such a web hosted file, only the File is created.
The good part is that in your app, you don't have to care if a File is referencing to a naee's hosted file or an external one. It just gives you the data when you request it!

Let's see some examples:
{% method %}
### Upload some data and create a File document
{% sample lang=“swift” %}
```swift
let url = ...
let file = File(fromURL: url)
file.save { error in
	if error == nil {
		// url content is upload and a File is created
		print(file.id!) 
	}
}
```
{% sample lang=“kotlin” %}
```kotlin
val url = ...
val file = File(url)
file.save { error ->
	if (error == null) {
		// url content is upload and a File is created
		println(file.id!) 
	}
}
```
{% sample lang=“java” %}
```java
Uri url = ...
File file = new File(url);
file.save( new FileCallback( { file in
	if (file != null) {
		// url content is upload and a File is created
	}
});
```
{% endmethod %}
{% method %}
### Create a File document referencing an external url
{% sample lang=“swift” %}
```swift
let url = ...
let file = File(fromExternalURL: url)
file.save { error in
	if error == nil {
		// A File is created, referencing to the provided URL
		print(file.id!) 
	}
}
```
{% endmethod %}
{% method %}
### Get the data referenced by a File document
{% sample lang=“swift” %}
```swift
let file = ..
file.fetchContent { data, error in 

	if let data = data {
		// Data are available to your app
	}
}
```
{% endmethod %}