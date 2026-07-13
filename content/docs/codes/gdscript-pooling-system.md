---
title: "GDScript - Pooling System"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
## Godot Engine: GDScript Pooling System

Inspired by [Object Pooling](https://www.unitydesignpatterns.com/patterns/objectpooling) script from [Unity Design Patterns](https://www.unitydesignpatterns.com/) by Gülnaz Gürbüz.  

The original is written in C# for Unity3D, here's the following code for Godot Engine:

(If you prefer it on Github or suggest a change in the code, here's the [link](https://github.com/Falme/godot-engine-pooling-system/tree/main))


```gdscript
# Filename "object_pool.gd" 

class_name ObjectPool
extends Node

@export var _model: PackedScene
@export var _initial_pool_size: int

# Spawn initial objects at once or one per frame?
@export var _spawn_initial_pool_at_once: bool

var _pool: Array[Node]
var _current_initial_pool_index: int

# Initialize pool with initial number of items
func _ready() -> void:
	if _spawn_initial_pool_at_once:
		for i in _initial_pool_size:
			_create_item()

		_current_initial_pool_index = _initial_pool_size


func _process(_delta: float) -> void:
	if _current_initial_pool_index < _initial_pool_size:
		_create_item()
		_current_initial_pool_index += 1


func get_item() -> Node:
	if _pool.size() <= 0:
		_create_item()

	var item: Node = _pool.pop_back()
	_show_item(item)
	remove_child(item)
	return item


func return_item(item: Node) -> void:
	# make item an orphan before changing parents
	item.get_parent().remove_child(item)
	add_child(item)
	_hide_item(item)
	_pool.push_back(item)


func _create_item() -> Node:
	var new_item: Node = _model.instantiate()

	# If you need to send this class (ObjectPool) through Dependency Injection
	# Create an "init(pool: ObjectPool)" function inside the item class
	if new_item.has_method("init"):
		new_item.init(self)

	add_child(new_item)
	_pool.push_back(new_item)
	return new_item


func _show_item(item: Node) -> void:
	if item is Node2D:
		item.show()


func _hide_item(item: Node) -> void:
	if item is Node2D:
		item.hide()
```

---

## How to use it?

This script can be used in an empty Node (usually named 'ObjectPool' or similar). It can only handle one object type as a Resource.

1. Create a Node in the scene
2. Add the script `object_pool` to the created Node
3. Add the object to be created in the `Model` field
4. Add the number of objects to be created at the start of the game in the `Initial Pool Size` field
5. Set if you want to spawn the objects at the start of the game at once or per frame in the `Spawn Initial Pool at Once` field
	- True: It will spawn the objects in a quantity defined at `Initial Pool Size` all at once in the first frame
	- False: It will spawn the objects in a quantity defined at `Initial Pool Size` one per frame

All the objects that's waiting to be used will be stored inside the 'ObjectPool' Node, when you call an object to be used with the `get_item()` function, it will change parents and become visible.
