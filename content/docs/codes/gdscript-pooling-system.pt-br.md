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

## Como usar?

Este script pode ser usado em um Node vazio (geralmente chamado de 'ObjectPool' ou similar). Ele só pode lidar com um tipo de objeto como Resource.

1. Crie um Node na cena.
2. Adicione o script `object_pool` ao Node criado.
3. Adicione o objeto a ser criado no campo `Model`.
4. Adicione o número de objetos a serem criados no início do jogo no campo `Initial Pool Size`.
5. Defina se deseja gerar os objetos no início do jogo de uma vez só ou por cada frame no campo `Spawn Initial Pool at Once`.
	- True: Isso irá gerar os objetos em uma quantidade definida em `Initial Pool Size` todos de uma vez no primeiro frame.
	- False: Isso irá gerar os objetos em uma quantidade definida em `Initial Pool Size`, um para cada frame.

Todos os objetos que estão aguardando para serem usados serão armazenados dentro do Node 'ObjectPool'. Quando você chamar um objeto para ser usado com a função `get_item()`, ele mudará de pai e se tornará visível.
