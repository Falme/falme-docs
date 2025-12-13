---
title: "Bandit"
# weight: 0
# bookFlatSection: false
# bookToc: true
bookHidden: true 
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit

Olá, bem vindes a minha jornada no CTF do Over The Wire, um dos meus CTFs 
favoritos. 

Eu já completei ele anos atrás, mas retornei para revisitar e dessa 
vez documentar como que eu resolvo esses desafios.

Lembrando que essa documentação é para mim, e que se você não resolveu esse ctf 
por conta própria, não leia essas postagens, vá e resolva os desafios por conta 
própria, não estrague essa experiência.

---

## Level 00

> The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

Traduzindo de uma forma livre, ficaria algo como:

> O objetivo desse level é que você entre no sistema do jogo usando SSH. O host que você precisa conectar é o bandit.labs.overthewire.org, na porta 2220. O usuário é bandit0 e a senha é bandit0. Uma vez logado, vá para a página Level 1 para descobrir como vencer o Level 1.

Esse level é a introdução ao sistema que iremos nos aventurar: Bandit.

Então a solução está no problema, ele te diz qual o login e senha para entrar 
no jogo. O único problema aqui é como usar o comando SSH.

Nesse caso ficaria algo como:

```shell
ssh bandit.labs.overthewire.org -p 2220 -l bandit0
```

Onde:
- `ssh` : É a chamada do comando SSH
- `bandit.labs.overthewire.org` : é o host a se conectar
- `-p 2220` : é a porta a se conectar, no caso do bandit é sempre 2220
- `-l bandit0` : é o nome de usuário que está conectando, nesse caso, bandit0.

Sendo assim, terminamos o level0. Agora podemos ir para o Level1.

---

## Level 00 -> Level 01

Daqui pra frente, os desafios irão realmente começar. Normalmente eu uso a tag 
de spoiler para o resultado da flag, mas no caso do OverTheWire, a senha muda 
ocasionalmente, então eu só vou fazer um passo-a-passo do problema sem uma 
resposta final.

### Objetivo
> The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

Em uma tradução livre seria algo como:

> A senha para o próximo level está guardada num arquivo chamado readme 
localizado na pasta home. Use essa senha para logar no usuário bandit1 usando 
SSH. Quando você encontrar uma senha para um level, use o SSH (na porta 2220) 
para logar naquele level e continuar o jogo.

### Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find


### Passos

Esse level tem a intenção de ensinar os basicos de como usar um terminal unix. 
No meu caso eu estou usando um Linux numa Máquina Virtual.

No último level que estivemos (level 0) nós ganhamos acesso a um usuário 
chamado bandit0. Nós podemos ver isso na identificação do shell para o usuário 
que está atualmente logado:

```shell
bandit0@bandit:~$
```

Lendo o objetivo desse desafio, nossa senha está num arquivo chamado `readme`. 
Então primeiro vamos listar todos os arquivos da pasta atual usando o comando 
`ls`. E só nos mostrou um arquivo chamado `readme`. Esse é o arquivo que contém 
a senha pro próximo level.

Para ler o conteúdo do arquivo nós iremos usar um comando chamado `cat`, ele 
vai nos mostrar o conteúdo do arquivo.

Com isso, nós temos um texto com a senha para o próximo level.

---

## Level 01 -> Level 02

### Objetivo
> The password for the next level is stored in a file called - located in the home directory

Em uma tradução livre, seria algo como:

> A senha para o próximo level está guardada num arquivo chamado - localizado no diretório home

### Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

### Passos

Nesse caso o arquivo se chama literalmente `-`, você pode listar o arquivo no 
diretório usando o comando `ls` e pode averiguar que o arquivo está mesmo lá.

Mas não é possível só usar o comando `cat -` pra ler o arquivo. Entenda que, o 
caractere traço é um caractere especial, ele é usado para definir opções em um 
programa. Vamos ver outro exemplo:

```shell
ls -l 
```

Nesse caso, o comando `ls` está usando a opção `-l`, na qual lista os arquivos 
em um formato de lista longa, com mais detalhes.

Então não podemos usar `cat -`, porque cat está selecionando uma opção a ser 
executada. Pra fazer isso funcionar nós precisamos de uma alternativa (e assim 
a razão pra esse desafio), porque cat está esperando um input de um arquivo, 
nós vamos usar um pouco de redirecionamento Input/Output (I/O redirection).

Primeiro vamos explicar o que é Input/Output redirection. Se nós fizermos algo 
como `echo "test text" > file.txt`, isso significa que nós iremos escrever no 
console usando o comando `echo` o texto `"test text"`, e então redirecionar o 
Output para um arquivo chamado `file.txt` usando o sinal `>`.

Ok, então se `>` significa output (saída), o `<` significa input (entrada), 
certo?

Então vamos colocar o arquivo `-` dentro do comando `cat`, será algo como:

```shell
cat < -
```

E com isso, nós temos a senha para o próximo level.

---

## Level 02 -> Level 03

### Objetivo
> The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory

Em uma tradução livre, ficaria algo como:

> A senha para o próximo level está guardada num arquivo chamado `--spaces in this filename--` localizado no diretório home

### Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

### Passos

Esse aqui é parecido com o level anterior (level 2), mas nesse caso o caractere 
especial é o espaço. Normalmente usar o espaço depois de chamar um programa é 
entendido como separador de argumentos. Então, vamos ver um exemplo usando `cat`:

```shell
cat file.txt
```

Então nesse caso nós temos o programa `cat` e um argumento esperado, separado 
por um espaço, um arquivo com o nome `file.txt`.

Lendo um arquivo que tem espaço no nome vai causar confusão no console, ele não 
consegue separar o que é espaço e o que é um argumento. Pra fazer isso 
funcionar, supreendentemente, temos duas opções pra ler o arquivo.

#### Aspas

Nós podemos usar áspas ou áspas duplas em volta do caminho do arquivo, sendo 
reconhecido como espaço entre as aspas e argumentos fora das aspas. Seria algo 
como:

```shell
cat < "--spaces in this filename--"
```

(O `<` é necessário, pois `-` também é um caractere especial)

#### Barra Invertida

A Barra Invertida também é um caractere especial, mas é usado para "Escapar um 
caractere", fazendo um espaço ser lido literalmente como um espaço e não um 
separador. Seria algo como:

```shell
cat < --spaces\ in\ this\ filename--
```

Então cada espaço do caminho do arquivo deve ser substituído por Barra 
Invertida+Espaço. Dessa maneira a senha para o próximo level é revelada.

---

## Level 03 -> Level 04

### Objetivo
> The password for the next level is stored in a hidden file in the `inhere` directory.

Numa tradução livre, seria algo como:

> A senha para o próximo level está guardado em um arquivo escondido no diretório `inhere`.

### Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

### Passos

Esse desafio nos ensina sobre arquivos e diretórios escondidos (hidden). No 
Linux, arquivos escondidos são marcados com um ponto (`.`) antes do nome do 
arquivo. Isso faz com que ele seja marcado como escondido e não irá aparecer 
em um comando `ls` comum.

Então se nós usarmos `ls` no diretório home, nós podemos encontrar o diretório 
`inhere`. Então nós entramos no `inhere` usando o comando `cd`:

```shell
cd inhere
```

Agora, dentro do diretório `inhere`, se listarmos os arquivos usando `ls`, 
nenhum arquivos vai ser encontrado. Para mostrar arquivos escondidos em um 
diretório nós vamos usar a opção `-a` para mostrar todos os arquivos, 
incluindo os arquivos escondidos. Será algo como:

```shell
ls -a
```

Com isso, o arquivo `...Hiding-From-You` é revelado, lemos o arquivo com `cat`: 

```shell
cat < ...Hiding-From-You
```

E a senha é revelada para o próximo level.

---

## Level 04 -> Level 05

### Objetivo
> The password for the next level is stored in the only human-readable file in the `inhere` directory. Tip: if your terminal is messed up, try the “reset” command.

Numa tradução livre, seria algo como:

> A senha para o próximo level está armazenada no único arquivo legível para humanos no diretório `inhere`. Dica: se o seu terminal estiver com problemas, tente o comando "reset".

### Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

### Passos

(Eu sei o que acontece no próximo level, então vamos jogar de forma mais simples 
nesse aqui)

O objetivo é encontrar um arquivo human-readable (legível para humanos) no diretório `inhere` contendo a senha para o próximo nível. Isso significa que o arquivo não está em código de máquina. 

O código de máquina é ilegível quando você tenta lê-lo por texto, então vamos tentar encontrar um arquivo de texto simples para leitura. Primeiro, entramos na pasta `inhere` com `cd`:

```shell
cd inhere
```

Listando os arquivos da pasta com o comando `ls`, temos a seguinte lista de arquivos:

```
-file00
-file01
-file02
-file03
-file04
-file05
-file06
-file07
-file08
-file09
```

Desta vez, usaremos o comando `file`, que exibe os detalhes e informações do arquivo. O problema é que `file < -file00` não funciona, precisamos usar outra forma para exibir seu conteúdo no console, sem chamar uma opção. Então usaremos `--`.

```shell
file -- -file00
```

Assim, usando o comando `file` em cada um dos arquivos, temos as seguintes informações:

```
-file00: data
-file01: OpenPGP Public Key
-file02: OpenPGP Public Key
-file03: data
-file04: data
-file05: data
-file06: data
-file07: ASCII text
-file08: data
-file09: data
```

As informações que reunimos são as seguintes:

- data : código de máquina, não legível para humanos
- OpenPGP Public Key : chave para criptografia, não legível para humanos
- ASCII text : Padrão de codificação de caracteres, legível para humanos

Com isso, temos o arquivo que queremos, `-file07`, e lemos seu conteúdo com o comando `cat`:

```shell
cat < -file07
```

Resultando na senha para o próximo level.

---

## Level 05 -> Level 06

### Objetivo
> The password for the next level is stored in a file somewhere under the `inhere` directory and has all of the following properties:
>
>    human-readable  
>    1033 bytes in size  
>    not executable  

Numa tradução livre, seria algo como:

> A senha para o próximo level está armazenada em um arquivo em algum lugar no diretório `inhere` e possui todas as seguintes propriedades:
>
>    human-readable (legível para humanos)  
>    1033 bytes in size (1033 bytes em tamanho)  
>    not executable (não executável)  

### Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

### Passos

Desta vez, vamos usar o poder do Linux e o comando `find`. Para este desafio, 
recomendo que você leia o manual do comando `find`, usando `man find`, isso 
será muito útil.

Estamos procurando um arquivo legível para humanos, com 1033 bytes e não 
executável. Para isso, usaremos o comando `find` com alguns parâmetros de 
filtragem. Mas primeiro, para testar, vamos apenas listar todos os arquivos, 
nem precisa abrir e entrar na pasta `inhere`:

```shell
find ./
```

Esse comando vai listar todos os arquivos na pasta atual (onde `./` significa 
pasta atual) de forma recursiva. O que precisamos agora é filtrar os resultados 
para que correspondam exatamente aos parâmetros que procuramos. Consultando o 
manual do `find`, encontramos a opção `-executable`:

```
-executable
              Corresponde a arquivos executáveis e diretórios pesquisáveis (no 
              sentido da resolução do nome do arquivo) pelo usuário atual.
```

Ok, agora podemos filtrar os resultados para mostrar apenas os arquivos 
executáveis, porém, não é isso que procuramos. Estamos procurando arquivos que 
NÃO sejam executáveis. Voltando ao manual, podemos encontrar a opção `-not`, 
que inverte o resultado de uma opção:

```
-not expr
              Mesmo que ! expr, mas não compatível com POSIX.
```

Utilizando essas opções, podemos listar todos os arquivos não executáveis 
dentro de `inhere`:

```shell
find ./ -not -executable
```

Mas ainda assim, ainda tem muitos arquivos para pesquisar manualmente; 
precisamos filtrar melhor. Desta vez, filtraremos por tamanho, procurando por 
arquivos que tenham exatamente 1033 bytes. Consultando novamente o manual do 
comando `find`, encontramos a opção `-size`:

```
-size n[cwbkMG]
              O arquivo usa menos, mais ou exatamente n unidades de espaço, 
              arredondando para cima. Os seguintes sufixos podem ser usados:

              'c'    para bytes
```

Com a opção de tamanho, podemos adicioná-la ao nosso comando e verificar os 
resultados:

```shell
find ./ -not -executable -size 1033c
```

Com este comando, o resultado é filtrado para apenas um arquivo:

```
./inhere/maybehere07/.file2
```

Ao ler o arquivo com o comando `cat`, a senha para o próximo level é revelada.

---

## Level 06 -> Level 07

### Objetivo
> The password for the next level is stored somewhere on the server and has all of the following properties:
>
>    owned by user bandit7  
>    owned by group bandit6  
>    33 bytes in size  

Numa tradução livre, seria algo como:

> A senha para o próximo level está armazenada em algum lugar no servidor e possui todas as seguintes propriedades:
>
>    pertence ao usuário bandit7  
>    pertence ao grupo bandit6  
>    33 bytes de tamanho  

### Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

### Passos

Este desafio é uma continuação do level anterior (nível 6), a solução é praticamente a mesma de antes, mas com alguns parâmetros diferentes.

Primeiramente, o objetivo é buscar um arquivo em algum lugar do servidor, o que significa que não estamos limitados ao diretório `inhere` ou diretório home. Portanto, estaremos procurando nos arquivos em `/`, que é o diretório raiz, e a partir daí, podemos encontrar recursivamente todos os possíveis arquivos no sistema.

Então, vamos começar com as opções do comando `find` que já conhecemos, ou seja, a opção `-size`:

```shell
find / -size 33c
```

Este comando irá percorrer todo o sistema tentando encontrar um arquivo com 33 bytes. Os resultados mostram muitos arquivos, então precisamos filtrar melhor para encontrar o arquivo correto. Consultando o manual, podemos encontrar a opção `-user`:

```
-user nome-usuário
    O arquivo pertence ao usuário uname (ID numérico do usuário permitido). 
```

Este comando permite filtrar os arquivos que pertence ao usuário bandit7.

```shell
find / -size 33c -user bandit7
```

Ainda estão sendo exibidos muitos arquivos possíveis, então precisamos filtrar também por grupo. Consultando o manual do comando `find`, encontramos a opção `-group`:

```
-group nome-grupo
    O arquivo pertence ao grupo nome-grupo (ID numérico do grupo permitido).
```

Este comando permite filtrar os arquivos do grupo bandit6.

```shell
find / -size 33c -user bandit7 -group bandit6
```

Com isso, você já pode encontrar o arquivo com a senha para o próximo level. Mas, há muitos arquivos nos resultados aos quais não temos permissão de acesso. Então, vamos fazer uma limpeza para que apenas o arquivo correto seja exibido.

#### File Descriptors

Já aprendemos sobre redirecionamento com os caracteres especiais `<` e `>`, agora vamos aprender sobre descritores de arquivo (File Descriptors). Os diferentes tipos de File Descriptors são:

- stdin: Representado pelo número 0, é a entrada.
- stdout: Representado pelo número 1, é a saída output comum.
- stderr: Representado pelo número 2, é a saída output de erro.

No nosso caso, queremos remover os erros da saída do nosso comando `find`. Portanto, vamos usar stderr (2) no exemplo.

Reescrevendo o comando anterior, estamos adicionando o seguinte redirecionamento:

```shell
2>/dev/null
```

Explicando um pouco a estrutura:

- `2`: é o stderr, ou erro, que o comando irá gerar, como uma tag da saída.
`>` : é o redirecionamento, queremos que as mensagens de erro vão para outro lugar, qualquer lugar que não seja o nosso console.
- `/dev/null`: é o caminho para um arquivo especial, usado para descartar informações, neste caso, queremos descartar os erros.

E com isso descartamos as mensagens de erro. No final, o comando final será algo como:

```shell
find / -size 33c -user bandit7 -group bandit6 2>/dev/null
```

Com o resultado, o arquivo correto é revelado. Usando o comando `cat` para ler o arquivo, vai encontrar a senha para o próximo level.

---

## Level 07 -> Level 08

### Objetivo
> The password for the next level is stored in the file data.txt next to the word millionth

Numa tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt, ao lado da palavra millionth.

### Comandos que você pode precisar para resolver esse level
> man, grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Passos

Este desafio tem como objetivo nos ensinar o comando `grep`. O comando `grep` mostra linhas que correspondem a um padrão, neste exemplo, uma string.

Imagine que você está procurando em um arquivo grande e precisa de uma palavra específica; o comando grep ignorará as outras linhas e mostrará apenas aquela que você precisa.

Por que isso é necessário, você pergunta? Em primeiro lugar, é prático. Em segundo lugar, você pode automatizar algumas buscas a partir da linha de comando, mas em terceiro lugar, e o mais importante para mim, é para encontrar padrões em arquivos enormes.

Alguns arquivos, como listas e dicionários, são apenas arquivos de texto (.txt) com gigabytes de tamanho. Se você tentar abri-los com o Bloco de Notas, não vai funcionar. Então, para brincar com ele, vamos voltar ao Objetivo.

Queremos encontrar a palavra "millionth", então, usando o comando `grep`, ficaria algo como:

```shell
cat data.txt | grep 'millionth'
```

Onde:
- `cat`: abre o arquivo
- `data.txt`: o arquivo para ler os dados
- `|` : caractere especial de pipe, que envia a saída de um comando para a entrada de outro comando.
- `grep`: comando para filtrar a saída de texto de `cat data.txt`
- "millionth": a string que queremos filtrar

Com isso, a senha para o próximo level aparecerá ao lado da palavra "millionth".

---

## Level 08 -> Level 09

### Objetivo
> The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

Numa tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt e é a única linha de texto que aparece apenas uma vez.

### Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Passos

Precisamos encontrar uma linha única no arquivo data.txt que não se repete, porque esse arquivo está repleto de possíveis senhas. O primeiro instinto é usar o comando `uniq`, pois, em sua descrição:

```
uniq - reportar ou omitir linhas repetidas
```

E é correto usar o comando `uniq`, que mostra apenas as linhas que são únicas e não repetidas. Mas, no final da página do manual do `uniq`, temos:

```
'uniq' não detecta linhas repetidas a menos que sejam adjacentes.
       Você pode querer ordenar a entrada primeiro ou usar 'sort -u' sem
       'uniq'.
```

Então, antes de usarmos o comando `uniq` para filtrar as linhas repetidas, precisamos ordenar as linhas alfabeticamente e, em seguida, usar o comando `uniq`. O comando será algo como:

```shell
sort data.txt | uniq -u
```

Where:
- `sort`: ordena as linhas alfabeticamente.
- `data.txt`: arquivo a ser lido e processado
- `|` : caractere especial de pipe, que envia o conteúdo de `data.txt` para o comando `uniq`
- `uniq`: comando para reportar ou omitir linhas repetidas
- `-u`: opção do `uniq` para mostrar apenas linhas únicas

Com isso, apenas a senha para o próximo level será mostrada.

---

## Level 09 -> Level 10

### Objetivo
> The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several '=' characters.

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt em uma das poucas sequências legíveis para humanos, precedida por vários caracteres '='.

### Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Passos

A maior parte do arquivo `data.txt` está preenchida com código de máquina, o que significa que não é legível por humanos. Poderíamos usar o comando `xxd` para ler o arquivo e procurar a senha manualmente, mas há muito conteúdo e isso levaria muito tempo.

Então, vamos pensar da seguinte forma: precisamos apenas da parte legível do arquivo, ou seja, apenas as strings contidas nele. Portanto, usaremos o comando `strings`, que exibe apenas as strings dentro de um arquivo, nada mais. O resultado será algo como:

```shell
strings data.txt
```

Só serão exibidas as strings do arquivo. Mas ainda é muito conteúdo. No texto do objetivo, diz que a senha é precedida por vários caracteres '=', então vamos usar o comando `grep` para filtrar esses caracteres. Mas quantos '=' são considerados "vários"? Vamos tentar 2, parece suficiente para a busca. O resultado será algo como:

```shell
strings data.txt | grep '=='
```

E com isso, a senha para o próximo level é revelada.

---

## Level 10 -> Level 11

### Objetivo
> The password for the next level is stored in the file data.txt, which contains base64 encoded data

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt, que contém dados codificados em base64.

### Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Passos

Este exercício é o primeiro que aborda (e nos ensina) sobre criptografia. Não que precisamos entender criptografia, mas precisamos descriptografar um arquivo. O arquivo `data.txt` está criptografado em base64, e nós possuímos o comando `base64`. No manual do `base64`, temos a opção `-d`:

```
-d, --decode
              decodificar dados
```

Então, usamos essa opção para decodificar o arquivo `data.txt`, ficaria mais ou menos assim:

```shell
base64 -d data.txt
```

E com isso, a senha para o próximo level é revelada.

---

## Level 11 -> Level 12

### Objetivo
> The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt, onde todas as letras minúsculas (a-z) e maiúsculas (A-Z) foram rotacionadas em 13 posições.

### Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd

### Passos

Este desafio é semelhante ao anterior (level 11), mas desta vez não estamos lidando com a criptografia base64, e sim com a criptografia ROT13. 

A criptografia ROT13 é bastante simples: ela desloca um caractere 13 letras da posição inicial, como em uma rotação. Se o caractere for 'A', ele será traduzido para 'N'; se for 'B', será traduzido para 'O', e assim por diante. Para uma demonstração, veja a imagem abaixo:

![Image description](/images/picogym-mod-26/1.jpg)

Para o nosso desafio, usaremos um comando chamado `tr`, que traduz caracteres de um caractere para outro. Com alguns exemplos, ficará muito fácil de entender. Vamos traduzir um caractere da palavra `test`:

```shell
echo "test" | tr "s" "N"
```

No exemplo acima, estamos usando `tr` para converter o caractere 's' em 'N'. O resultado será `teNt`, com N em maiúsculo, exatamente como escrevemos. Mas não é apenas um único caractere que pode ser traduzido, mas também uma sequência de caracteres:

```shell
echo "test" | tr "r-u" "N"
```

No caso acima, estamos traduzindo os caracteres de 'r' a 'u' (inclusivo) para o caractere 'N'. O resultado será `NeNN` porque 't' e 's' estão entre 'r' e 'u' no alfabeto.

Você deve estar se perguntando se ambos possuem um argumento de intervalo. Vamos analisar:

```shell
echo "test" | tr "r-u" "N-Q"
```

No caso acima, estamos traduzindo os caracteres [r,s,t,u] para os caracteres [N,O,P,Q]. O resultado será `PeOP`, porque tentarão encontrar a correspondência entre suas posições em ambos os arrays para obter o resultado. O 't' está na mesma posição que 'P' e 's' está na mesma posição que 'O'.

Agora podemos fazer algumas alterações para concluir nosso desafio, executando o seguinte comando:

```shell
cat data.txt | tr "N-ZA-Mn-za-m" "A-Za-z"
```

Where:
- `cat data.txt`: irá ler o arquivo ROT13 codificado
- `|` : irá direcionar o resultado de `data.txt` para o comando `tr`
- `tr`: irá traduzir os caracteres codificados para caracteres decodificados.
- `"N-ZA-Mn-za-m"` : são os caracteres originais que queremos alterar.
	- O array de caracteres terá uma aparência semelhante a esta:
	- [N, O, P, Q, ..., Y, Z, A, B, C, ..., L, M, n, o, p, q, ..., y, z, a, b, c, ..., l, m]
	- Em outras palavras, um alfabeto deslocado em maiúsculas e minúsculas.
- `"A-Za-z"`: é a transformação dos caracteres, então ficará mais ou menos assim:
	- [A, B, C, ..., Y, Z, a, b, c, ..., y, z]

O primeiro array será substituído pelo segundo array, realizando uma conversão ROT13 bem sucedida. Agora você tem a senha para o próximo level.

---

## Level 12 -> Level 13

### Objetivo
> The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada no arquivo data.txt, que é um dump hexadecimal de um arquivo que foi compactado repetidamente. Para este level, pode ser útil criar um diretório em /tmp onde você possa trabalhar. Use o comando mkdir com um nome de diretório difícil de adivinhar. Ou melhor, use o comando “mktemp -d”. Em seguida, copie o arquivo de dados usando cp e renomeie-o usando mv (leia as páginas do manual!).

### Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file

### Passos

Esse desafio foi meio estranho para mim, não sei se deixei passar alguma coisa que o tornaria mais fácil, mas foi um pouco chato de terminá-lo.

A primeira coisa a fazer é ler o arquivo `data.txt` com o comando `cat`. Trata-se de um arquivo de texto contendo um hexdump. Um hexdump é um despejo de dados de um arquivo binário para texto, exibindo mais informações sobre o arquivo, provavelmente feito com o comando `xxd`.

A boa notícia é que o arquivo pode ser revertido para o binário original usando o comando `xxd` com a opção `-r`:

```
-r | -revert
    Operação inversa: converter (ou corrigir) um dump hexadecimal em binário.
```

Mas não estaremos fazendo isso dentro da pasta pessoal, estamos gerenciando vários arquivos, então vamos criar uma pasta temporária e trabalhar dentro dela. Para isso, usaremos o comando `mktemp -d` para criar uma pasta temporária e entraremos nela com `cd`.

Em seguida, copiamos o arquivo do diretório inicial para o diretório temporário. Usaremos o comando `cp` para copiar o arquivo da home para o novo diretório:

```shell
cp ~/data.txt /tmp/temporary-folder/
```

Em seguida, podemos reverter o hexdump para um arquivo binário com `xxd -r`:

```shell
xxd -r data.txt data.bin
```

#### Descompressão de Sísifo

A partir de agora, teremos uma lista de tarefas para repetir até obtermos nossa senha:

1. `file`: para identificar o tipo de compressão do arquivo.
	- Se o tipo de arquivo for gzip, renomeie o arquivo para terminar com .gz usando o comando `mv data.bin data.gz`.
2. Extraia o arquivo do tipo de arquivo correto:
	- gzip: use o comando `gunzip` para extrair o arquivo gzip.
	- tar: use o comando `tar -xvf` para extrair o arquivo tar.
	- bzip2: use o comando `bzip2 -d` para extrair o arquivo bzip2.
3. repetir

Repetindo o processo algumas vezes, você encontrará um arquivo ASCII com a senha para o próximo level.

---

## Level 13 -> Level 14

### Objetivo
> The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada em /etc/bandit_pass/bandit14 e só pode ser lida pelo usuário bandit14. Para este level, você não recebe a próxima senha, mas sim uma chave SSH privada que pode ser usada para acessar o próximo level. Analise os comandos que permitiram o acesso aos níveis anteriores do bandit e descubra como usar a chave para este level.

### Comandos que você pode precisar para resolver esse level
> ssh, scp, umask, chmod, cat, nc, install

### Passos

Normalmente, acessamos um level usando o comando `ssh` com uma solicitação de senha, mas desta vez vamos usar uma chave privada. Uma conexão segura usando chaves pública e privada está além do escopo deste guia, mas posso garantir que a chave privada não deve ser acessível a outros usuários. Essa é a sacada deste desafio: a chave privada é acessível e vamos usá-la para acessar o próximo level.

Na home, podemos encontrar o arquivo `sshkey.private`. Essa é uma chave privada RSA e podemos ler seu conteúdo com o comando `cat`, mostrando todo seu conteúdo criptografado. Vamos copiar essa chave privada e salvá-la em nossa máquina em um novo arquivo chamado `id_rsa`.

Agora, fora do usuário bandit, vamos nos conectar ao próximo level apenas com a chave RSA. O primeiro lugar que o `ssh` verifica em busca de chaves é no caminho `~/.ssh`, então vamos colocar nossa chave privada roubada lá.

Depois disso, basta conectar-se via `ssh` para o próximo level, sem necessidade de senha.

#### WARNING: UNPROTECTED PRIVATE KEY FILE!

Se você vir esta mensagem, o problema é que a chave privada tem permissões em excesso. Por ser um arquivo privado, ter permissões demais representa um problema de segurança e a chave privada será ignorada. Para resolver isso, altere as permissões do arquivo da chave privada usando o comando `chmod`.

```shell
chmod 600 id_rsa
```

Onde:
- `chmod`: comando para alterar permissões
- `600`: tipo de permissão de arquivo, normalmente separada em Usuário, Grupo e Outros
	- `6` (Usuário): Ler e Escrever
	- `0` (Grupo): Sem permissão
	- `0` (Outros): Sem permissão
- `id_rsa`: arquivo da chave privada

Agora, dentro do próximo level, você tem acesso à senha em /etc/bandit_pass/bandit14.

---

## Level 14 -> Level 15

### Objetivo
> The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

Em uma tradução livre, seria algo como:

> A senha para o próximo level pode ser obtida enviando a senha do level atual para a porta 30000 no localhost.

### Comandos que você pode precisar para resolver esse level
> ssh, telnet, nc, openssl, s_client, nmap

### Passos

Entendendo o texto do objetivo, a porta 30000 está em escuta (listening), aguardando a senha do level atual. Para estabelecer uma conexão TCP/UDP através de uma porta em escuta, utilizaremos o comando `nc`. O comando `nc` (ou netcat) foi criado para esse tipo de tarefa, portanto, vamos estabelecer a conexão com:

```shell
nc localhost 30000
```

Nenhuma resposta será exibida no console, mas ele está aguardando a entrada. Basta digitar a senha do level atual, pressionar Enter e a senha para o próximo level será exibida.

---

## Level 15 -> Level 16

### Objetivo
> The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
> 
> Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

Em uma tradução livre, seria algo como:

> A senha para o próximo level pode ser obtida enviando a senha do level atual para a porta 30001 no localhost usando criptografia SSL/TLS.
> 
> Dica: Está recebendo os comandos “DONE”, “RENEGOTIATING” ou “KEYUPDATE”? Leia a seção “CONNECTED COMMANDS” do manual.

### Comandos que você pode precisar para resolver esse level
> ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

### Passos

Este desafio é muito semelhante ao anterior (level 15), mas desta vez precisamos estabelecer uma conexão segura usando criptografia SSL/TLS. Para isso, precisamos de um Netcat mais poderoso, então usaremos o comando `ncat` com a opção `--ssl`:

```
--ssl             Conecte-se ou escute com SSL
```

Estabeleceremos uma conexão segura com o localhost na porta 30001, da seguinte forma:

```shell
ncat --ssl localhost 30001
```

Após estabelecer a conexão, inserimos a senha do level atual e pressionamos Enter. Com isso, a senha para o próximo level é revelada.

---

## Level 16 -> Level 17

### Objetivo 
> The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
> 
> Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

Em uma tradução livre, seria algo como:

> As credenciais para o próximo level podem ser obtidas enviando a senha do level atual para uma porta no localhost no intervalo de 31000 a 32000. Primeiro, descubra quais dessas portas têm um servidor escutando. Em seguida, descubra quais deles usam SSL/TLS e quais não usam. Há apenas um servidor que fornecerá as próximas credenciais; os outros simplesmente retornarão o que você enviar para eles.
> 
> Nota útil: Está recebendo os comandos “DONE”, “RENEGOTIATING” ou “KEYUPDATE”? Leia a seção “CONNECTED COMMANDS” na página do manual.

### Comandos que você pode precisar para resolver esse level
> ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss

### Passos

Neste desafio, a forma de resolvê-lo é a mesma do level anterior (level 16), mas a porta à qual precisamos nos conectar é desconhecida em um intervalo de 1000 portas (de 31000 a 32000). Precisamos encontrar a porta correta e nos conectar a ela usando o comando `ncat --ssl`.

Para encontrar portas abertas em um intervalo, usaremos uma ferramenta chamada `nmap`. Essa ferramenta é um scanner de várias coisas, incluindo portas. Usaremos o `nmap` para escanear essas 1000 portas e verificar quais estão abertas e quais estão fechadas. O comando será algo como:

```shell
nmap localhost -p 31000-32000
```

Onde:
- `nmap`: é a ferramenta de varredura
- `localhost`: é o endereço do servidor
- `-p`: é a opção para procurar em portas específicas.
- `31000-32000`: é o intervalo de portas a serem verificadas, o hífen significa "entre"

Com isso, temos o resultado de 5 portas abertas, mas sem nenhuma informação. Sabemos que a porta correta aceita apenas conexões SSL, então usaremos o mesmo comando que usamos antes com o `ncat`:

```shell
ncat --ssl localhost {port}
```

Normalmente, uma porta incorreta resultará em um erro, e a porta correta não retornará nada. Basta inserir a senha do level atual e o sistema retornará as credenciais RSA do próximo level.

Salve as credenciais em um novo arquivo e use o comando `ssh` para se conectar sem precisar de uma senha:

```shell
ssh -i ./key-saved.private bandit.labs.overthewire.org -p 2220 -l bandit17
```

Se tiver alguma dúvida, confira o desafio do level 14.

---

## Level 17 -> Level 18

### Objetivo 
> There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
> 
> NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

Em uma tradução livre, seria algo como:

> Existem dois arquivos no diretório inicial: passwords.old e passwords.new. A senha para o próximo level está em passwords.new e é a única linha que foi alterada entre passwords.old e passwords.new.
> 
> NOTA: se você concluiu este level e ver um "Byebye!" ao tentar entrar no bandit18, isso está relacionado ao próximo level, bandit19.

### Comandos que você pode precisar para resolver esse level
> cat, grep, ls, diff

### Passos


Este desafio tem como objetivo nos ensinar sobre o comando `diff`. O `diff` compara dois arquivos e mostra a diferença entre eles, sendo muito usado com o `git` e outros programas de controle de versão. A ideia é que, dado um arquivo antigo e um novo, o comando mostrará o que mudou entre os dois.

O caractere "<" representa uma diferença que saiu do arquivo, e o caractere ">" representa uma diferença que entrou no arquivo. Então, vamos comparar os arquivos:

```shell
diff passwords.old passwords.new
```

A senha para o próximo level deve estar ao lado do caractere ">".

---

## Level 18 -> Level 19

### Objetivo
> The password for the next level is stored in a file readme in the home directory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

Em uma tradução livre, seria algo como:

> A senha para o próximo level está armazenada em um arquivo readme no diretório home. Infelizmente, alguém modificou o arquivo .bashrc para te desconectar quando você faz login via SSH.

### Comandos que você pode precisar para resolver esse level
> ssh, ls, cat

### Passos

Este desafio te desconecta logo após a conexão com o usuário bandit18. O arquivo .bashrc inicializa um script que mostra o texto "Byebye!" e desconecta você. 

A solução para isso é injetar um comando via SSH antes que você seja desconectado. Felizmente, o comando `ssh` aceita um comando injetado dentro da sequência de conexão. É algo como:

```shell
ssh bandit.labs.overthewire.org -p 2220 -l bandit18 "cat ~/readme"
```

O comando `"cat ~/readme"` é um comando comum do console e será executado logo após o estabelecimento da conexão. Ele serve para ler o conteúdo do arquivo `readme`.

Com isso, a senha para o próximo level é revelada.

---

## Level 19 -> Level 20

### Objetivo 
> To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

Em uma tradução livre, seria algo como:

> Para acessar o próximo level, você deve usar o binário setuid no diretório home. Execute-o sem argumentos para descobrir como usá-lo. A senha para este level pode ser encontrada no local usual (/etc/bandit_pass), após você ter usado o binário setuid.

### Comandos que você pode precisar para resolver esse level
> 

### Passos

Existe um arquivo executável na pasta inicial chamado `bandit20-do`. Para saber mais sobre esse executável, vamos executá-lo sem argumentos:

```shell 
./bandit20-do
```

Isso nos retorna o seguinte texto:

```
Run a command as another user. (Executar um comando como outro usuário.)
	Example: ./bandit20-do whoami
```

O comando `whoami` mostra o nome do usuário atualmente conectado. Se usarmos apenas o comando `whoami`, ele retornará "bandit19", o usuário ao qual nos conectamos via `ssh`. Mas se executarmos `./bandit20-do whoami`, ele retornará "bandit20".

Portanto, o que concluímos disso é:

- O executável bandit20-do pode receber outros comandos
- bandit20-do age como o usuário bandit20

Com essa informação, usaremos o programa `bandit20-do` para ler a senha do bandit20 em /etc/bandit_pass/bandit20:

```shell
./bandit20-do cat /etc/bandit_pass/bandit20
```

Com isso, temos a senha para o próximo level.

---

## Level 20 -> Level 21

### Objetivo
> There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
> 
> NOTE: Try connecting to your own network daemon to see if it works as you think

Em uma tradução livre, seria algo como:

> Existe um binário setuid no diretório home que faz o seguinte: ele estabelece uma conexão com o localhost na porta que você especificar no argumento da linha de comando. Em seguida, lê uma linha de texto da conexão e a compara com a senha do level anterior (bandit20). Se a senha estiver correta, ele transmitirá a senha para o próximo level (bandit21).
> 
> NOTA: Tente conectar-se ao seu próprio daemon de rede para ver se funciona como você espera.

### Comandos que você pode precisar para resolver esse level
> ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)

### Passos

Este desafio nos ensina sobre as conexões entre dois locais que o netcat pode fazer (não muito neste caso, porque ambos estarão rodando no localhost, mas enfim...). 

Então, temos um novo executável em nosso diretório home chamado `suconnect`, que se conecta a uma porta e aguarda a chegada de informações. Para facilitar o entendimento, vamos abrir dois consoles, ambos conectados ao usuário "bandit20" (se preferir usar o `tmux` ou similar, sem problemas). Vamos chamá-los de "ConsoleA" e "ConsoleB".

No "ConsoleA", iniciaremos uma conexão de escuta do netcat em uma porta aleatória. Digamos que usamos a porta `4321`, que ficará mais ou menos assim:

```shell 
nc -l localhost 4321
```

Onde:
- `nc`: programa netcat para conexão
- `-l`: opção de escuta, para que possamos aguardar a nova senha.
- `localhost`: servidor para escutar
- `4321`: porta aleatória

Agora o "ConsoleA" está executando o netcat no modo de escuta. Agora, vamos para o "ConsoleB".

No "ConsoleB", vamos executar o programa `suconnect` na mesma porta que o netcat. Será algo como:

```shell
./suconnect 4321
```

Agora, ambos os consoles estarão ativos, só precisamos processar isso. Isso funcionará da seguinte maneira:

1. Inserimos a senha de bandit20 no "ConsoleA", que está executando o netcat.
2. O "ConsoleB", executando o `suconnect`, receberá a string do netcat.
3. O "ConsoleB", executando o `suconnect`, verificará se a senha está correta.
4. Se a senha estiver correta, uma nova senha será enviada de volta para "ConsoleA", que está executando o netcat.

Com isso, a senha para o próximo level é revelada.

---

## Level 21 -> Level 22

### Objetivo
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

Em uma tradução livre, seria algo como:

> Um programa está sendo executado automaticamente em intervalos regulares pelo cron, o agendador de tarefas baseado em tempo. Dê uma olhada em /etc/cron.d/ para achar a configuração e qual comando está sendo executado.

### Comandos que você pode precisar para resolver esse level
> cron, crontab, crontab(5) (use “man 5 crontab” to access this)

### Passos

Desta vez, vamos analisar os arquivos de configuração do `cron`. O `cron` é um agendador de tarefas que roda em segundo plano para executar tarefas periodicamente. As tarefas podem ser encontradas no caminho /etc/cron.d/, então vamos dar uma olhada lá.

Encontramos alguns arquivos, mas no momento o arquivo `cronjob_bandit22` é o mais adequado ao nosso objetivo. Usando o comando `cat` para ler o arquivo, podemos encontrar o seguinte caminho:

```
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

Sabemos que /dev/null é um arquivo de descarte especial, mas pode haver algo no script /usr/bin/cronjob_bandit22.sh que nos ajude. Portanto, usamos `cat` novamente para verificar o que ele faz.

O programa grava periodicamente a senha de /etc/bandit_pass/bandit22 em um arquivo na pasta /tmp/. Ao abrir esse arquivo na pasta /tmp/, a senha para o próximo nível é revelada.

---

## Level 22 -> Level 23

### Objetivo 
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
> 
> NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

Em uma tradução livre, seria algo como:

> Um programa está sendo executado automaticamente em intervalos regulares pelo cron, o agendador de tarefas baseado em tempo. Verifique o arquivo /etc/cron.d/ para ver a configuração e qual comando está sendo executado.
> 
> NOTA: Analisar scripts de shell escritos por outras pessoas é uma habilidade muito útil. O script para este level foi intencionalmente desenvolvido para ser fácil de ler. Se você estiver com dificuldades para entender o que ele faz, tente executá-lo para ver as informações de depuração que ele imprime.

### Comandos que você pode precisar para resolver esse level
> cron, crontab, crontab(5) (use “man 5 crontab” to access this)

### Passos

Os passos para acessar o script que está sendo executado na rotina `cron` são os mesmos do level anterior (level 22):

- Abra a pasta /etc/cron.d/ com o comando `cd`.
- Leia o conteúdo do arquivo `cronjob_bandit23`
- Acesse o caminho do script dentro dele (`cronjob_bandit23.sh`)
- Leia o script 

Este é um script bastante simples: ele obtém o usuário atualmente logado com o comando `whoami`, codifica o resultado com um codificador `md5sum` e remove os caracteres desnecessários. O resultado dessa codificação é o nome do arquivo na pasta /tmp/ que contém a senha para o próximo level.

Para encontrar o resultado, basta repetir corretamente os passos do script até chegar ao caminho do arquivo correto. É algo como:

```shell 
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

Where
- `echo I am user bandit23` : Escreve uma string para ser redirecionada para `md5sum` e bandit23 é o usuário simulado.
- `md5sum`: codifica em MD5 a última string
- `cut -d ' ' -f 1`: remove os caracteres extras desnecessários

Ao ler o arquivo no caminho encontrado, a senha do próximo level é revelada.

---

## Level 23 -> Level 24

### Objetivo 
> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
> 
> NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
> 
> NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

Em uma tradução livre, seria algo como:

> Um programa está sendo executado automaticamente em intervalos regulares pelo cron, o agendador de tarefas baseado em tempo. Verifique o arquivo /etc/cron.d/ para ver a configuração e qual comando está sendo executado.
> 
> NOTA: Este level exige que você crie seu primeiro script shell. Este é um passo muito importante e você deve se orgulhar de si mesmo ao concluir este level!
> 
> NOTA 2: Lembre-se de que seu script shell é removido após a execução, portanto, talvez seja interessante manter uma cópia...

### Comandos que você pode precisar para resolver esse level
> chmod, cron, crontab, crontab(5) (use “man 5 crontab” to access this)

### Passos

A forma de encontrar o script dentro da pasta cron.d é a mesma dos dois níveis anteriores (level 22 e level 23). Depois de encontrar o script `cronjob_bandit24.sh`, analise-o. Se não entender alguma parte, verifique o bloco de código comentado a seguir:

```shell 
#!/bin/bash # Executar como um script bash

myname=$(whoami) # salva na variável "myname" o usuário atual

cd /var/spool/$myname/foo # Entra em um diretório chamado "foo"
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*; # Percorre todos os arquivos da pasta.
do
    if [ "$i" != "." -a "$i" != ".." ]; # Se não for um diretório
    then
        echo "Handling $i" # Script atual que está lidando
        owner="$(stat --format "%U" ./$i)" # Obtenha o nome do dono do script.
        if [ "${owner}" = "bandit23" ]; then # se o dono for bandit23
            timeout -s 9 60 ./$i # Executa um script e feche se ultrapassar 1 minuto.
        fi
        rm -f ./$i # Deleta o script
    fi
done
```

Em outras palavras, existe um script que é executado periodicamente, verificando o diretório `/var/spool/bandit24/foo/` em busca de outros scripts e executando-os caso o dono seja bandit23. Após a execução, o script é excluído.

A solução para esse problema é copiar um script shell personalizado para o diretório `/var/spool/bandit24/foo/` que pega a senha para o próximo level. Primeiro, vamos criar um script shell chamado `getpass.sh` em uma pasta temporária usando o comando `mktemp -d`. Você pode editar o texto no seu editor de texto preferido (nano, vim, etc...).

O script que escrevi é o seguinte:

```shell 
#!/bin/bash

result=$(cat /etc/bandit_pass/bandit24)
echo "res" > /tmp/tmp.tempfolderhash/$result
```

Onde:
- `#!/bin/bash` : instrui o executor a usar o bash
- `result=$(cat /etc/bandit_pass/bandit24)` : salva a senha do bandit24 em uma variável chamada "result"
- `echo "res" > /tmp/tmp.tempfolderhash/$result`: salva um arquivo na pasta temporária com a senha como nome do arquivo, assim como no script de level anterior. ("res" é um argumento de descarte para o echo. Você poderia usar `touch`)

Agora, altere as permissões para que outros usuários possam executar o script usando `chmod a+x getpass.sh`, e copie o script para `/var/spool/bandit24/foo/` usando o comando `cp`:

```shell
cp ./getpass.sh /var/spool/bandit24/foo/getpass.sh
```

Agora, aguarde um pouco e a senha para o próximo level aparecerá como um arquivo no diretório temporário que você criou.

---

## Level 24 -> Level 25

### Objetivo
> A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
> You do not need to create new connections each time

Em uma tradução livre, seria algo como:

> Um daemon está escutando na porta 30002 e fornecerá a senha para bandit25 se você fornecer a senha para bandit24 e um código PIN numérico secreto de 4 dígitos. Não há como recuperar o código PIN, exceto testando todas as 10.000 combinações, um processo conhecido como força bruta.
> Você não precisa criar novas conexões a cada vez.

### Comandos que você pode precisar para resolver esse level
> 

### Passos

Para este desafio, usaremos o `ncat` para a conexão na porta 30002 e um script para gerar um texto. A conexão é bastante simples:

```shell
ncat localhost 30002
```

Isso se conecta a um daemon, que aguarda a combinação senha_bandit24+pincode. Mas tentar uma por uma levaria uma eternidade. Uma vantagem é que o `ncat` pode receber um arquivo de texto com entradas e processá-lo normalmente. Então, imagine que temos o seguinte arquivo `list.txt`:

```
senha_bandit24 0
senha_bandit24 1
senha_bandit24 2
senha_bandit24 3
```

Se inserirmos o arquivo desta forma:

```shell
ncat localhost 30002 < .list.txt
```

Cada linha será processada como uma entrada comum do usuário. Bom, ok, precisamos escrever 10.000 linhas assim. Isso também levaria uma eternidade. Então, vamos criar um script shell para fazer isso por nós. Vamos criar um script chamado `create_list.sh`, torná-lo executável com `chmod +x create_list.sh` e editá-lo com algo como isto:

```shell
for i in {0..10000}
do
echo "senha_bandit24 $i" >> ./list.txt
done
```

Onde:
- `for i in {0..10000}`: é um loop que se repete de 0 a 10000, salvando a posição atual na variável `$i`.
- `echo "password_bandit24 $i"` : imprime a string com a senha + código PIN da variável
- `>> ./list.txt`: adiciona uma nova linha a um arquivo, neste caso no arquivo `list.txt`, a cada interação do loop.

Execute este script com `./create_list.sh` e o arquivo conterá todas as linhas necessárias para encontrar a próxima senha. Agora, para obter a próxima senha, basta executar `ncat` com a lista preenchida.

```shell
ncat localhost 30002 < .list.txt
```

---

## Level 25 -> Level 26

### Objetivo
> Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
> 
>    NOTE: if you’re a Windows user and typically use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

Em uma tradução livre, seria algo como:

> Fazer login no usuário bandit26 a partir do bandit25 deve ser relativamente fácil… O shell do usuário bandit26 não é /bin/bash, mas sim outro. Descubra qual é, como funciona e como escapar dele.
> 
>    NOTA: Se você usa o Windows e costuma usar o PowerShell para acessar o Bandit via SSH: o PowerShell pode causar problemas com a solução proposta para este nível. Recomenda-se usar o prompt de comando.

### Comandos que você pode precisar para resolver esse level
> ssh, cat, more, vi, ls, id, pwd

### Passos

Este me deu trabalho na primeira vez que o resolvi; você precisa pensar fora da caixa, ou melhor, pensar no tamanho da caixa.

O problema é que, na pasta pessoal, existe uma chave privada RSA para fazer login no usuário bandit26, mas após o login, a conexão é interrompida. Precisamos verificar outras coisas primeiro.

O texto do objetivo diz que bandit26 não está usando /bin/bash como shell, então precisamos verificar qual shell ele está usando. Para isso, precisamos verificar as informações de todos os usuários do sistema. Vamos verificar o arquivo `passwd`.

```shell 
cat /etc/passwd | grep bandit26
```

Este arquivo contém o caminho de um script shell chamado `showtext`, e ao exibir seu conteúdo com `cat`, revela-se a simplicidade do script. Ele apenas configura um ambiente, imprime o texto da apresentação na tela e encerra o terminal.

O truque está na linha que lê o texto da apresentação. Ela usa o comando `more`, e nós vamos usar isso a nosso favor.

Primeiro, reduza bastante a altura do terminal, para cerca de 2 ou 3 linhas, e então conecte-se normalmente ao usuário bandit26. Metade do texto da apresentação deve aparecer, pois o comando `more` pausará a execução do script, aguardando a entrada do usuário.

Neste momento, pressione a tecla `v`, isso entrará no modo de edição/visual do Vim. Agora você está em um editor de texto. Vamos abrir a senha para o nível bandit26. Digite o seguinte no Vim:

```shell 
:e /etc/bandit_pass/bandit26
```

Isso abrirá o arquivo diretamente no terminal. Copie a senha para o próximo nível e feche o vim com `:qa` (sair todos os arquivos).

---

## Level 26 -> Level 27

### Objetivo
> Good job getting a shell! Now hurry and grab the password for bandit27!

Em uma tradução livre, seria algo como:

> Ótimo trabalho por ter conseguido entrar no shell! Agora se apresse e pegue a senha do bandit27!

### Comandos que você pode precisar para resolver esse level
> ls

### Passos

Ainda temos problemas não resolvidos para o bandit26, então precisamos repetir os mesmos passos do level anterior (level 26) até que o modo visual do Vim seja ativado (após pressionar `v` para editar).

Nesse momento, precisamos executar um programa chamado `bandit-27-do`, mas não conseguimos acessá-lo com o terminal atual. Portanto, vamos abrir um novo terminal dentro do editor vim com o comando:

```
:term bash
```

Com este comando, vamos abrir um shell bash separado. Agora podemos executar `bandit27-do`, que é semelhante a `bandit20-do`, simulando que somos o bandit27 e executando um comando. Então, abrimos o shell com a senha do bandit27 usando:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

Agora, a senha para o próximo level foi revelada.

---

## Level 27 -> Level 28

### Objetivo 
> There is a git repository at ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo através da porta 2220. A senha para o usuário bandit27-git é a mesma do usuário bandit27.
> 
> Clone o repositório e encontre a senha para o próximo level.

### Comandos que você pode precisar para resolver esse level
> git

### Passos

A partir de agora, vamos brincar com o `git`. O Git é um programa de controle de versão que usa repositórios para rastrear as alterações em um projeto, criando uma linha do tempo das mudanças de forma eficaz e com ótimo desempenho.

Este é o básico para lidar com o `git`. Vamos clonar um repositório para uma pasta, o que significa fazer uma cópia do servidor para a nossa máquina. O comando será algo como:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

Lembre-se de que estamos lidando com uma porta diferente (2220), a porta SSH comum é 22, então precisamos adicioná-la ao endereço SSH.

Após clonar o repositório, encontre o arquivo README, ele contém a senha para o próximo level.

---

## Level 28 -> Level 29

### Objetivo 
> There is a git repository at ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo através da porta 2220. A senha para o usuário bandit28-git é a mesma do usuário bandit28.
> 
> Clone o repositório e encontre a senha para o próximo level.

### Comandos que você pode precisar para resolver esse level
> git

### Passos

Agora, faça o mesmo que no level anterior (level 28) para clonar o repositório em uma pasta, mas agora para o bandit28-git.

Depois disso, podemos verificar se o arquivo `README` não contém a senha para o próximo level. Lembra que eu disse que o `git` mantém um histórico das alterações no projeto? Bem, chegou a hora de verificar o que foi alterado. Vamos executar o comando:

```bash
git show
```

Este comando mostrará quais foram as alterações no arquivo `README` e também revelará a senha para o próximo level.

---

## Level 29 -> Level 30

### Objetivo 
> There is a git repository at ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo através da porta 2220. A senha para o usuário bandit29-git é a mesma do usuário bandit29.
> 
> Clone o repositório e encontre a senha para o próximo level.

### Comandos que você pode precisar para resolver esse level
> git

### Passos

Clone o repositório da mesma forma que no level anterior e vamos encontrar a senha para este desafio.

Desta vez, o desafio nos ensina sobre branches. No `git`, branches são diferentes linhas do tempo de um projeto. Digamos que você queira testar uma funcionalidade, então, você cria uma nova branch para testá-la de forma não destrutiva. Primeiro, vamos verificar quantas branches estão disponíveis. Podemos fazer isso com:

```shell 
git branch --all
```

Isso mostrará todas as branches disponíveis no projeto. Vamos verificar a branch de desenvolvimento chamada `origin/dev`. Para mudar para essa branch, usaremos o comando `git checkout`:

```shell 
git checkout origin/dev 
```

Agora, mudamos da branch master para origin/dev, e ao abrir o arquivo README.md com o comando `cat`, a senha para o próximo level é exibida.

---

## Level 30 -> Level 31

### Objetivo
> There is a git repository at ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo através da porta 2220. A senha para o usuário bandit30-git é a mesma do usuário bandit30.
> 
> Clone o repositório e encontre a senha para o próximo level.

### Comandos que você pode precisar para resolver esse level
> git

### Passos

Primeiramente, clone o repositório como nos níveis anteriores. Depois disso, podemos continuar a jornada para encontrar a senha do próximo level.

Desta vez, o `git log` e o `git show` mostram apenas um arquivo, sem alterações. Parece que chegamos a um beco sem saída. Talvez tenha um branch diferente, mas nenhum branch nos leva à senha. 

Na verdade, a resposta está no sistema de arquivos dentro da pasta oculta `.git`, que mostra alguns arquivos compactados. Então, vamos verificar as referências dos pacotes no repositório. Abra os arquivos `.git/packed-refs` com o comando `cat` para ver uma referência curiosa:

```
843...351 /refs/tags/secret
```

O primeiro hash provavelmente é o hash de um commit de uma tag chamada "secret". Portanto, usaremos um comando chamado `git cherry-pick` para obter o commit específico da árvore.

```
git cherry-pick 843...351
```

Então, o `git` nos retorna a mensagem: "Não é possível selecionar um blob com cherry-pick". Ok, isso significa que não estamos lidando com um commit, mas com um blob de algum tipo. Consultando o manual do `git`, encontramos um comando que resolve isso, chamado `unpack-file`, que diz:

```
git-unpack-file(1)
           Cria um arquivo temporário com o conteúdo de um blob.
```

Então, vamos executar o seguinte comando:

```shell 
git unpack-file 843...351
```

Depois disso, um arquivo chamado `.merge_file_uEpLe8` será criado na mesma pasta. Em seguida, use o comando `cat` para ler o arquivo descompactado e revelar a senha para o próximo level.

---

## Level 31 -> Level 32

### Objetivo
> There is a git repository at ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.
> 
> Clone the repository and find the password for the next level.

Em uma tradução livre, seria algo como:

> Existe um repositório git em ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo através da porta 2220. A senha para o usuário bandit31-git é a mesma do usuário bandit31.
> 
> Clone o repositório e encontre a senha para o próximo level.

### Comandos que você pode precisar para resolver esse level
> git

### Passos

Primeiramente, clone o repositório como nos níveis anteriores. Depois disso, podemos continuar a jornada para encontrar a senha do próximo level.

Neste desafio, vamos aprender sobre o arquivo `.gitignore`, esse arquivo define o que NÃO deve ser rastreado no projeto. Pode ser um arquivo binário, um arquivo que você não quer enviar para o repositório, etc.

Após clonar o repositório, você pode encontrar o arquivo `.gitignore` e lê-lo com o comando `cat`. Ele mostrará que todos os arquivos "*.txt" estão sendo ignorados, portanto, nenhum arquivo .txt será rastreado.

Depois, há o arquivo README.md, cujo texto diz:

```
This time your task is to push a file to the remote repository.
(Desta vez, sua tarefa é enviar um arquivo para o repositório remoto.)

Details:
	File name: key.txt
	Content: 'May I come in?'
	Branch: master
```

Portanto, precisamos criar um arquivo chamado `key.txt` com o texto "May I come in?" no branch master. Porém, o arquivo `.gitignore` não rastreia arquivos .txt, então não podemos enviar esse arquivo.

Então, vamos excluir o arquivo .gitignore e confirmar nossas alterações. Sem o .gitignore, nada nos impede de enviar o arquivo .txt.

Então, para enviar uma alteração no `git`, geralmente seguimos esses passos:

- `git add ./file`: Adicionamos o arquivo ao stage, preparando-o para o commit.
- `git commit -m "mensagem"`: Confirmamos nossas alterações e arquivos, geralmente também escrevemos uma mensagem para saber o que mudou.
- `git push --all`: Em seguida, enviamos nossas alterações e arquivos para o servidor onde o repositório está localizado.

Com o novo arquivo, ficará mais ou menos assim:

```shell
git add ./key.txt
git commit -m "May I come in?"
git push --all
```

Se você fez tudo certo, o `git` retornará um erro com a senha para o próximo level. 

---

## Level 32 -> Level 33

### Objetivo
> After all this git stuff, it’s time for another escape. Good luck!

Em uma tradução livre, seria algo como:

> Depois de toda essa coisa de Git, é hora de outra escapada. Boa sorte!

### Comandos que você pode precisar para resolver esse level
> sh, man

### Passos

Este é o último desafio disponível no momento. Trata-se de mais uma escapada do shell para obter a senha dentro do diretório /etc/bandit_pass/.

O shell em que você está preso converte todos os comandos para letras maiúsculas, o que é um problema para o Linux, pois os comandos diferenciam maiúsculas de minúsculas, e você não consegue digitar nenhum comando sem receber um erro. 

A solução que encontrei foi escrever `$0`, uma das variáveis especiais que executa o shell atual, mas neste caso ela redireciona para outro tipo de shell, que não está mais restrito a letras maiúsculas. Dentro deste novo shell, escreva `bash` para carregar o terminal bash.

Agora, livres do terminal em maiúsculas, podemos ir para `/etc/bandit_pass/bandit33` e ler a senha do próximo level com `cat`.

---

