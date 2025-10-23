---
title: "GDScript Cheatsheet"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
## Godot Engine: GDScript Cheatsheet

### Basics

Comments
```gdscript
# this is a comment

"""
This is
a multi-line
comment
"""
```

Variables
```gdscript
# structure : var [name] : [type] = [value]
var this_is_a_variable : int = 10
```

Functions
```gdscript
func this_is_a_function() -> void:
	print("example")
```


### Built In Basic Node Functions

Call a Init Method (Similar to Start() in Unity)
```gdscript
func _ready():
	print("Method Ready Called")
```

Call a game loop Method (Similar to Update() with Time.deltaTime in Unity)
```gdscript
func _process(delta):
	print("Method Process with Delta Called")
```

Call a game Physics loop Method (In Unity is like FixedUpdate() with Time.fixedDeltaTime )
```gdscript
func _physics_process(delta):
	print("Method Physics Process with Delta Called")
```


### Public and Private Nomenclature

Apply an _ (underscore) for private methods, and none for public methods
```gdscript
# Private
var _my_private_variable : int = 10
func _my_private_function()
	pass

# Public
var my_public_variable : int = 10
func my_public_function()
	pass
```


### Scene Management

Change Scene
```gdscript
get_tree().change_scene_to_file('res://'+nextScene+'.tscn')
```

Reload Scene
```gdscript
get_tree().reload_current_scene()
```

Quit Game
```gdscript
get_tree().quit()
```


### Arrays

Initialize Arrays
```gdscript
var _local_variable: Array[String] = []
```

Resize Arrays
```gdscript
var _local_variable: Array[String] = []
_local_variable.resize(10) # now it has length = 10
```


### Node Management

Show and hide object
```gdscript
# hide Node
selected_node.hide()

# show Node
selected_node.show()
```


### Engine Interface Management

@export calls in the editor a reference to a variable (Similar to \[SerializeField\] in Unity)
```gdscript
@export var animation_player_path : NodePath
```

Set a custom script icon
```gdscript
# Must have an image for the icon and call it this way
@icon("res://Theme/Icons/vibration.png")
class_name VibrationArea2D extends Node
```


### Signals

Call an existing signal to be emmited
```gdscript
# structure : [node].[signal_name].connect([function_to_be_called])
timer.timeout.connect(_on_timer_timeout)
```


