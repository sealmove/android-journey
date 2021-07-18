# android-journey
Memos from experimenting with native android app development

### Making pixels density-independent
`TypedValue.applyDimension` knows about the pixel density of the device.
It's important to always use it wherever you use literal pixel values, otherwise the sizes will be displayed differently from device to device.
Here is a helper function to reduce boilerplate:
```kotlin
private fun dipToPx(x: Float) =
    TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, x, resources.displayMetrics).toInt()
```
