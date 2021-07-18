# android-journey
Memos from experimenting with native android app development

- [String resources](#string-resources)
- [Making pixels density-independent](#making-pixels-density-independent)
- [Positioning elements relative to each other](#positioning-elements-relative-to-each-other)

## String resources
Wherever there is a hardcoded string in your code, it should be moved to an xml file. You can reference string resources stored in xml files in the `res` directory using the static `R` class.

#### `strings.xml`
```xml
<resources>
    <string name="my_button">press me</string>
</resources>
```

#### `MyActivity.kt`
```kotlin
val button = Button(this)
button.text = getString(R.string.my_button)
```

## Making pixels density-independent
`TypedValue.applyDimension` knows about the pixel density of the device.
It's important to always use it wherever you use literal pixel values, otherwise the sizes will be displayed differently from device to device.
Here is a helper function to reduce boilerplate:
```kotlin
private fun dipToPx(x: Float) =
    TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, x, resources.displayMetrics).toInt()
```

## Positioning elements relative to each other
A `RelativeLayout.LayoutParams` can be used as a second argument when adding a view to your layout.
The most common constructor accepts 2 integers, for the width and height.
For dynamic behavior, it's important to use the special constants `-1` and `-2` instead of an actual value. These are named `MATCH_PARENT` and `WRAP_CONTENT` respectively:
- **`MATCH_PARENT`**: Fills up the layout it's been placed in
- **`WRAP_CONTENT`**: Expands only far enough to show all its content

After constructing the object, you can add _rules_ to it. These rules may reference a view using its ID.
The modern way to generate IDs is `View.generateViewId()`.

In this example, `button2` will be placed above `button1`:
```kotlin
val button1 = Button(this)
button1.id = View.generateViewId()

val button2 = Button(this)

val details = RelativeLayout.LayoutParams(
    RelativeLayout.LayoutParams.WRAP_CONTENT,
    RelativeLayout.LayoutParams.WRAP_CONTENT)
details.addRule(RelativeLayout.ABOVE, button1.id)

mainLayout.addView(button1)
mainLayout.addView(button2, details)
```
