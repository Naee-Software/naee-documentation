# Install the SDK

## Swift SDK (iOS, macOS, tvOS, watchOS)

- Download the appropriate framework from [Swift Downloads](dev.naee.io/downloads/swift)
- Drag the naee-*os*.framework in your Xcode project. Check “Copy if needed” to be sure that the file is copied and not only referenced
- At the beginning of your source code files, insert `import Naee`

If you plan to use the UI components (if available for the platform), download the NaeeUI-*os*.framework and follow the instructions above, then user `import NaeeUI` statement in your code.

## Android SDK

- Download the Android library from [Android Downloads](dev.naee.io/downloads/android)
- Copy the naee-android.jar in your Android Studio project.
- At the beginning of your source code files, insert `import io.naee.*;`

## JavaScript SDK

- Download the JavaScript library from [JavaScript Downloads](dev.naee.io/downloads/javascript)
- Copy the naee-min.js in your web project.
- From plain HTML pages insert `<script src=‘naee-min.js’ />`
- From nodeJS sources insert  `var naee = require(‘naee-min.js’)`

