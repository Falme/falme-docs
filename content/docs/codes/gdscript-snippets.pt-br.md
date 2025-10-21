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
func ${1}(${2}) -> ${3}:
	pass
$0
endsnippet

# if conditional
snippet if
if ${1}:
	${2}
$0
endsnippet

# Input check by mapping
snippet input
if Input.is_action_just_pressed("${1}"):
	${2}
$0
endsnippet

# Change scene
snippet scene
get_tree().change_scene_to_file('res://${1}.tscn')
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
```


