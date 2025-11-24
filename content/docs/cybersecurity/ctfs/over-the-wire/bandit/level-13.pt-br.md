---
title: "Level 13"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 13

## Objetivo
> The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

Em uma tradução livre, seria algo como:

A senha para o próximo level está armazenada no arquivo data.txt, que é um dump hexadecimal de um arquivo que foi compactado repetidamente. Para este level, pode ser útil criar um diretório em /tmp onde você possa trabalhar. Use o comando mkdir com um nome de diretório difícil de adivinhar. Ou melhor, use o comando “mktemp -d”. Em seguida, copie o arquivo de dados usando cp e renomeie-o usando mv (leia as páginas do manual!).

## Comandos que você pode precisar para resolver esse level
> grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file

## Passos

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

### Descompressão de Sísifo

A partir de agora, teremos uma lista de tarefas para repetir até obtermos nossa senha:

1. `file`: para identificar o tipo de compressão do arquivo.
	- Se o tipo de arquivo for gzip, renomeie o arquivo para terminar com .gz usando o comando `mv data.bin data.gz`.
2. Extraia o arquivo do tipo de arquivo correto:
	- gzip: use o comando `gunzip` para extrair o arquivo gzip.
	- tar: use o comando `tar -xvf` para extrair o arquivo tar.
	- bzip2: use o comando `bzip2 -d` para extrair o arquivo bzip2.
3. repetir

Repetindo o processo algumas vezes, você encontrará um arquivo ASCII com a senha para o próximo level.


