---
title: "GDScript Snippets"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
## Godot Engine: GDScript Snippets


```gdscript
# GDScript snippets

# basic function
snippet f
func $1($2) -> $3:
	pass
$0
endsnippet

# if conditional
snippet if
if $1:
	$2
$0
endsnippet

# Input check by mapping
snippet input
if Input.is_action_just_pressed("$1"):
	$2
$0
endsnippet

# Change scene
snippet scene
get_tree().change_scene_to_file('res://$1.tscn')
$0
endsnippet

# Quit game
snippet quit
get_tree().quit()
$0
endsnippet

# Reload Scene
snippet reload
get_tree().reload_current_scene()
$0
endsnippet

# =======================================
#			GUT - Godot Unit Test
# =======================================

# Initialize test script
snippet gut_init
extends GutTest

class ${1:ClassName}_Exists:
	extends GutTest

	func test_${2:variable}_exists():
		var _$2 : $1 = $1.new()
		assert_not_null(_$2, "$1 should not be null")
		_$2.free()
$0
endsnippet

# Initialize class test
snippet gut_class
class Test${1:_Class_Method}:
	extends GutTest

	var _${3:variable} : ${2:Class}

	func before_each():
		_$3 = $2.new()

	func after_each():
		_$3.free()
	
	func test_${4:method_name}():
		assert_eq(1, 2, "should be equal")
$0
endsnippet

# Initialize gut test method
snippet gut_f
func test_$1():
	assert_eq(1, 2, "should be equal")
$0
endsnippet
```

