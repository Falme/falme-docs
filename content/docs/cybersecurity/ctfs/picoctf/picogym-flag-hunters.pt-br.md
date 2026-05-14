---
title: "PicoGym: Flag Hunters"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# PicoGym: Flag Hunters

> No momento, o PicoGym foi transferido para o CyLabAcademy. Alguns links podem estar diferentes.

Este desafio se chama "Flag Hunters" e nele analisaremos um código Python, identificaremos algumas falhas e encontraremos uma flag no programa conectado através do netcat `nc`.

- Dificuldade: Fácil
- Categoria: Engenharia Reversa
- Linguagem: Python 3.x

> Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print it? There might be something in it for you.  
>  
> Connect to the program with netcat:  
>  
> `$ nc verbal-sleep.picoctf.net 69420`

Ou numa tradução livre:


> A letra pula dos versos para o refrão como uma chamada de sub-rotinas. Há um refrão oculto que este programa não mostra por padrão. Você consegue fazê-lo imprimir esse refrão? Pode ser que ele seja útil para você.  
>  
> Conecte-se ao programa com o netcat:  
>  
> `$ nc verbal-sleep.picoctf.net 69420`


(O endereço e a porta de conexão podem ser diferentes dos seus)


## Como Resolver

Primeiro, vamos abrir o código-fonte `lyric-reader.py`:

```python
import re
import time

# Read in flag from file
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether’s ours to conquer, '''\
+ flag + '\n'

song_flag_hunters = secret_intro +\
'''

[REFRAIN]
We’re flag hunters in the ether, lighting up the grid,
[...]
We’re chasing that victory, and we’ll never quit.
CROWD (Singalong here!);
RETURN

[VERSE1]
Command line wizards, we’re starting it right,
[...]
Flag on the horizon, what challenge is next?

REFRAIN;

Echoes in memory, packets in trace,
[...]
Buried deep in the noise, but we will prevail.

REFRAIN;

Binary sorcerers, let’s tear it apart,
[...]
Patch it up right, and watch the lock release.

REFRAIN;

Ciphertext tumbling, breaking the spin,
[...]
Decrypt that flag and hear the ether call.

REFRAIN;

SQL injection, XSS flow,
[...]
In the world wide labyrinth, we’re never lost.

REFRAIN;

Stack's overflowing, breaking the chain,
[...]
Control the instruction, flags call my name.

REFRAIN;

END;
'''

MAX_LINES = 100

def reader(song, startLabel):
  lip = 0
  start = 0
  refrain = 0
  refrain_return = 0
  finished = False

  # Get list of lyric lines
  song_lines = song.splitlines()
  
  # Find startLabel, refrain and refrain return
  for i in range(0, len(song_lines)):
    if song_lines[i] == startLabel:
      start = i + 1
    elif song_lines[i] == '[REFRAIN]':
      refrain = i + 1
    elif song_lines[i] == 'RETURN':
      refrain_return = i

  # Print lyrics
  line_count = 0
  lip = start
  while not finished and line_count < MAX_LINES:
    line_count += 1
    for line in song_lines[lip].split(';'):
      if line == '' and song_lines[lip] != '':
        continue
      if line == 'REFRAIN':
        song_lines[refrain_return] = 'RETURN ' + str(lip + 1)
        lip = refrain
      elif re.match(r"CROWD.*", line):
        crowd = input('Crowd: ')
        song_lines[lip] = 'Crowd: ' + crowd
        lip += 1
      elif re.match(r"RETURN [0-9]+", line):
        lip = int(line.split()[1])
      elif line == 'END':
        finished = True
      else:
        print(line, flush=True)
        time.sleep(0.5)
        lip += 1



reader(song_flag_hunters, '[VERSE1]')
```

Parece ser uma canção lida verso por verso. Eles limitam o número de versos a serem cantados com:

```python
MAX_LINES = 100
```

Assim, após 100 linhas, o programa para, para garantir que não entre em um loop infinito dentro do `while`.

Internamente, ele divide as verificações `if` com o caractere ";" (ponto e vírgula), sendo as possíveis correspondências:

- 'REFRAIN': Retorna ao início de uma seção da letra
- 'CROWD': Verifica a entrada do usuário.
	- Normalmente, podemos encontrar uma vulnerabilidade no Python 2, onde a entrada pode ler uma variável, mas este não é o caso, pois estamos lidando com o Python 3.
- 'RETURN \[número\]': Retorna a uma linha específica
- 'END': Interrompe o canto/letra/verso. Também interrompe o programa.

E no início do programa, temos:

```python
# Read in flag from file
flag = open('flag.txt', 'r').read()

secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether’s ours to conquer, '''\
+ flag + '\n'

song_flag_hunters = secret_intro +\
```

Onde:
- `flag = open('flag.txt', 'r').read()`: Lê o arquivo flag.txt, onde reside nosso objetivo.
- `secret_intro`: É um parágrafo onde a flag é armazenada, mas não acessível.
- `song_flag_hunters`: São as letras da música que o usuário vê, mas com a variável `secret_intro` anexada à string.

Portanto, nossa string contém a flag, mas ela não está acessível ao usuário.

Se inserirmos a entrada na seção "CROWD" o valor `;END;`, o programa interpretará o ";" como uma nova linha e também reproduzirá a string "END" como um comando.

Isso fará com que o programa seja interrompido, encerrando sua execução.

Então, talvez possamos usar isso também com o comando `RETURN [0-9]`, para retornar a uma linha específica da letra da música. Vamos tentar retornar à linha '1', vou usar o comando `echo` para adicionar a string na conexão `nc`:

```bash
echo ";RETURN 1;" | nc verbal-sleep.picoctf.net 69420
```

Com isso, a letra retorna à parte `secret_intro`, revelando a flag.


{{% details title="Resposta: Flag" open=false %}}
```
picoCTF{70637h3r_f0r3v3r_befbccb7}
```
{{% /details %}}

