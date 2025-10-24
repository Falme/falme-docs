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

### Básicos

Comentários
```gdscript
# esse é um comentário

"""
Esse é um
comentário de
múltiplas linhas
"""
```

Variáveis
```gdscript
# estrutura : var [name] : [type] = [value]
var isso_e_uma_variavel : int = 10
```

Funções
```gdscript
func isso_e_uma_funcao() -> void:
	print("exemplo")
```

Condicionais / If-else
```gdscript
if true == false:
    print("If")
elif false == true:
    print("Else if")
else:
    print("Else")
```


### Funções Básicas sobre Node (Built In)

Chama o método inicial (Similar ao Start() na Unity)
```gdscript
func _ready():
	print("Método Ready foi chamado")
```

Chama a função de game loop (Similar ao Update() com Time.deltaTime na Unity)
```gdscript
func _process(delta):
	print("Método Process com Delta foi chamado")
```

Chama a função Physics Loop (Na Unity é como FixedUpdate() com Time.fixedDeltaTime)
```gdscript
func _physics_process(delta):
	print("Método Physics Process com Delta Time foi chamado")
```


### Nomenclatura Publica e Privada 

Aplique um _ (underline) para métodos privados, e nenhum para métodos públicos
```gdscript
# Privado
var _minha_variavel_privada : int = 10
func _minha_funcao_privada():
	pass

# Público
var minha_variavel_publica : int = 10
func minha_funcao_publica():
	pass
```


### Gerenciamento de Cena

Mudar cena
```gdscript
get_tree().change_scene_to_file('res://'+proxima_cena+'.tscn')
```

Recarregar cena
```gdscript
get_tree().reload_current_scene()
```

Fechar jogo
```gdscript
get_tree().quit()
```


### Arrays

Inicializar Arrays
```gdscript
var _variavel_local : Array[String] = []
```

Redimensionar Arrays
```gdscript
var _variavel_local : Array[String] = []
_variavel_local.resize(10) # agora o array tem length = 10
```


### Gerenciamento de Node

Mostrar e esconder objeto
```gdscript
# esconder Node
node_selecionado.hide()

# mostrar Node
node_selecionado.show()
```


### Gerenciamento da Interface da Engine

@export chama no editor uma referência para a variável (Similar ao \[SerializeField\] na Unity)
```gdscript
@export var animation_player_path : NodePath
```

Definir um ícone customizado ao script
```gdscript
# Deve ter uma imagem do ícone e chamar dessa forma:
@icon("res://Theme/Icons/vibration.png")
class_name VibrationArea2D extends Node
```


### Signals

Adicionar um signal a um script
```gdscript
# estrutura : signal [nome_do_signal]
signal timeout
```

Chama um signal existente pra ser emitido
```gdscript
# estrutura : [node].[nome_do_signal].connect([função_a_ser_chamada])
timer.timeout.connect(_on_timer_timeout)

func _on_timer_timeout() -> void:
    pass
```


