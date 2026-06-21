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

Inspirado no script [Object Pooling](https://www.unitydesignpatterns.com/patterns/objectpooling) de [Unity Design Patterns](https://www.unitydesignpatterns.com/) por Gülnaz Gürbüz.  

O script original foi escrito em C# para Unity3D, aqui está o código correspondente para o Godot Engine:

(Se preferir no Github ou sugerir uma alteração no código, aqui está o [link](https://github.com/Falme/godot-engine-pooling-system/tree/main))

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

