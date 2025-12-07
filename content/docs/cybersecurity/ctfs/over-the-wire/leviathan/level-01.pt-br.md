---
title: "Level 01"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan - Level 01

Este desafio tem 2 maneiras de ser resolvido: uma aleatória (a que eu resolvi primeiro) e outra não aleatória. Vamos conferir ambas.

## Passos

Após conectar com o usuário leviathan1 usando `ssh`, temos o diretório home com um arquivo executável chamado "check". Então, primeiro executamos para ver o que ele faz:

```shell
./check
```

Este programa pede uma senha e, se estiver incorreta, retorna uma mensagem de "adeus", encerrando o programa. Se a senha estiver correta, retorna algo que ainda não sabemos. Então, vamos ver como podemos resolver isso.

### Um Jeito Aleatório

Usando o comando `strings` para exibir todas as strings do programa, podemos ver que nada nos chama a atenção. Algumas palavras possíveis aqui e ali, mas nada promissor.

Então, podemos usar o comando `xxd` para exibir um hexdump com os valores lado a lado. E uma coisa que me chamou a atenção é que algumas palavras aparecem no hexdump, mas não no comando `strings`, uma delas é a palavra "sex". Ok, estranho, mas não custa tentar. Então, executando `./check` e digitando a palavra "sex", de fato, retornamos a um shell. A senha está correta.

### Um Jeito Não Aleatório

Antes de prosseguirmos para a próxima seção, vamos falar sobre outra maneira de alcançar o mesmo resultado, mas sem depender totalmente da sorte. Vamos usar o comando `ltrace`. Este comando rastreia as chamadas de biblioteca do programa para o sistema. Vamos verificar usando:

```shell
ltrace ./check
```

Um código estranho aparecerá, mas está funcionando e rastreando chamadas de biblioteca. Digitamos uma senha aleatória, como "asdf", e pressionamos Enter. O programa terminará, mas deixando alguns rastros, e um deles que realmente chama nossa atenção é a função `strcmp()` (String Compare / Comparação de Strings):

```
strcmp("asdf", "sex")
```

A função chamada está tentando comparar a string "asdf" com a string "sex". Elas não são iguais, então o programa reconhece como uma senha incorreta.

Com isso, podemos digitar a senha correta no programa. Digitando "sex" no campo de senha, somos redirecionados para um shell. Verificamos quem está logado com o comando `whoami`, o resultado é "leviathan2", então estamos logados como o usuário leviathan2.

Então, vamos pegar a senha para o próximo level no caminho `/etc/leviathan_pass/leviathan2`.

```shell
cat /etc/leviathan_pass/leviathan2
```

E agora, temos a senha para o próximo level.
