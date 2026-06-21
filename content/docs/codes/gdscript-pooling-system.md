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

var _pool: Array[Node]

# Initialize pool with initial number of items
func _ready() -> void:
	for i in _initial_pool_size:
		_create_item()


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
