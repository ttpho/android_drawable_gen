# Android Drawable Gen 
A Python script to auto-generate a Kotlin enum class for Android drawable resources, making it easier to reference drawables in a type-safe way.

### Features

- Scans your Android project's `res/drawable` folders for all drawable files.
- Generates a Kotlin enum class mapping each drawable to its resource ID.
- Outputs the generated code to a specified package under a `gen` folder.
- Includes helper functions for retrieving drawables by name.

### Usage
Run the script from your project root:

```
python3 android_drawable_gen.py com.example.yourapp
```

Or, if no argument is provided, it will prompt for the package name.

### Output
- Generates a file:
`app/src/main/java/<your/package>/gen/DrawableGen.kt`

- The file contains:
    - An enum class listing all drawables.
    - Helper functions for retrieving drawables by name.
### Key Functions
- `get_all_file_paths(folder_path)`: Recursively collects all file paths in a directory.
- `get_uppercase_name(filename)`: Converts a filename to uppercase with underscores.
- `convert_to_capitalized_class_name(input_string)`: Converts a string to CapitalizedCase.
- `write_file(filename, content)`: Writes content to a file.
- `create_child_folder(parent_path, child_folder_name)`: Ensures the output folder exists.
- `create_list_item_enum(all_files_drawable)`: Builds the enum entries for each drawable.
- `gen_drawable(package_name_app)`: Main function to generate the Kotlin file.

```
+-----------------------------+
|  Start (main)               |
+-----------------------------+
             |
             v
+-----------------------------+
| Get package name (arg/input)|
+-----------------------------+
             |
             v
+-----------------------------+
| gen_drawable(package_name)  |
+-----------------------------+
             |
             v
+-----------------------------+
| create_child_folder         |
| (for output .kt file)       |
+-----------------------------+
             |
             v
+-----------------------------+
| get_all_file_paths          |
| (from RES_FOLDER_PATH)      |
+-----------------------------+
             |
             v
+-----------------------------+
| Filter drawable files       |
+-----------------------------+
             |
             v
+-----------------------------+
| create_list_item_enum       |
| (build enum list)           |
+-----------------------------+
             |
             v
+-----------------------------+
| Fill CONTENT_FILE_TEMPLATE  |
| (replace placeholders)      |
+-----------------------------+
             |
             v
+-----------------------------+
| write_file (.kt output)     |
+-----------------------------+
             |
             v
+-----------------------------+
|           End               |
+-----------------------------+
```

### Requirements
- Python 3.x
- Run from the root of your Android project (expects res and java).

### Example
If your package is `com.example.myapp`, running:

```
python3 android_drawable_gen.py com.example.myapp
```
Will generate:
```
app/src/main/java/com/example/myapp/gen/DrawableGen.kt
```
With an enum like:

```kotlin
enum class DrawableGen(val fileName: String, val res: Int) {
    ICON_HOME("icon_home", R.drawable.icon_home),
    LOGO("logo", R.drawable.logo)
    // ...
}
```

And helper functions to retrieve drawables by name.

# Note 

Change 
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

To 

```kotlin
val context = LocalContext.current
val drawableId = remember(name) { DrawableGenHelper.getResDrawable(name).res }
Image(
    painterResource(id = drawableId),
    contentDescription = "..."
)
```
