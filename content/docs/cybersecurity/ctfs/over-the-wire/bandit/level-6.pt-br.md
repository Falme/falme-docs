---
title: "Level 6"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 6

## Objetivo
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

## Comandos que você pode precisar para resolver esse level
> ls , cd , cat , file , du , find

## Passos

Desta vez, vamos usar o poder do Linux e o comando `find`. Para este desafio, recomendo que você leia o manual do comando `find`, usando `man find`, isso será muito útil.

Estamos procurando um arquivo legível para humanos, com 1033 bytes e não executável. Para isso, usaremos o comando `find` com alguns parâmetros de filtragem. Mas primeiro, para testar, vamos apenas listar todos os arquivos, nem precisa abrir e entrar na pasta `inhere`:

```shell
find ./
```

Esse comando vai listar todos os arquivos na pasta atual (onde `./` significa pasta atual) de forma recursiva. O que precisamos agora é filtrar os resultados para que correspondam exatamente aos parâmetros que procuramos. Consultando o manual do `find`, encontramos a opção `-executable`:

```
-executable
              Corresponde a arquivos executáveis e diretórios pesquisáveis (no sentido da resolução do nome do arquivo) pelo usuário atual.
```

Ok, agora podemos filtrar os resultados para mostrar apenas os arquivos executáveis, porém, não é isso que procuramos. Estamos procurando arquivos que NÃO sejam executáveis. Voltando ao manual, podemos encontrar a opção `-not`, que inverte o resultado de uma opção:

```
-not expr
              Mesmo que ! expr, mas não compatível com POSIX.
```

Utilizando essas opções, podemos listar todos os arquivos não executáveis dentro de `inhere`:

```shell
find ./ -not -executable
```

Mas ainda assim, ainda tem muitos arquivos para pesquisar manualmente; precisamos filtrar melhor. Desta vez, filtraremos por tamanho, procurando por arquivos que tenham exatamente 1033 bytes. Consultando novamente o manual do comando `find`, encontramos a opção `-size`:

```
-size n[cwbkMG]
              O arquivo usa menos, mais ou exatamente n unidades de espaço, arredondando para cima. Os seguintes sufixos podem ser usados:

              'c'    para bytes
```

Com a opção de tamanho, podemos adicioná-la ao nosso comando e verificar os resultados:

```shell
find ./ -not -executable -size 1033c
```

Com este comando, o resultado é filtrado para apenas um arquivo:

```
./inhere/maybehere07/.file2
```

Ao ler o arquivo com o comando `cat`, a senha para o próximo level é revelada.

