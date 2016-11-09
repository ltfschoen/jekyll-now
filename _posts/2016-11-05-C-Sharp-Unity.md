---
layout: post
title: C# Unity 5 (in progress!)
---

**Note: Based on Ray Wenderlich course**
- https://www.raywenderlich.com/category/unity

# Table of Contents
  * [Chapter 1 - C# Unity 5](#chapter-1)

## Chapter 1 - C# Unity 5 <a id="chapter-1"></a>

### Variables
- Class variables
```
public string xyz = "abc";
...
```
- Local function variabes
```
string gameName = "PlayerAnxiety";
int anxiety = 10;
Debug.Log("Game: " + gameName + ", Anxiety: " + anxiety);
```

### Types
`int, float, char, string, datetime, bool`
`Vector3`
`float x = 4f;`

### Comments
`/**  **/`

### Operators
`++x, x++` (increment before/after assignment)

### Arrays
- Default values included
```
public int[] myArray = {1,2,3};
public int[] myArray = new int[3];
myArray[0] = 4
```


### Basic Script

- Change "Layout" to "2 by 3"

* Create object to attach script to
- Hierarchy > Create > 3D Object > cube

* Note: Script is a Component that can be customised with fields

* Create script on cube
- Select cube
- Inspect > Add Component > New Script > <script_name> (i.e. "Hello World" > C Sharp > Create and Add

* Move script into folder to organise
- Assets > New Folder > "Scripts" > (drag script into this folder)

* Save the Scene (level)
File > Save Scene > Main

* Open the script
- Double click script "Hello World" to open

* Events in script (respond to lifecycle events) are referenced by:
```
class ...
    public string abc = "Abc";
    void Start () {}
    void Update () {}
    void OnDisable () {
        Debug.Log ("Hello World");
    }
```

* Press "Play" button

* Disable the cube object 
- Inspector > Cube > Uncheck checkbox
- Note that public variables are editable in input fields

* View Console
- Window > Console (drag-and-drop into a view)

* Detach script from an object 
- Select say "cube" object,
- Right-click Settings icon in associated Script
- Select "Remove Component"

* Re-Attach script to object
- Drag-and-drop a Script from "Assets > Scripts folder" into "Hierarchy > Cube"
