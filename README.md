# android_drawable_gen
Python script gen enum class with drawable name and resource


# Note 

```kotlin
val context = LocalContext.current
val drawableId = remember(name) {
    context.resources.getIdentifier(
        name,
        "drawable",
        context.packageName
    )
}
Image(
    painterResource(id = drawableId),
    contentDescription = "..."
)
```
