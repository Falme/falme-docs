---
title: "Level 03"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan - Level 03

Já falei com vocês sobre a progressão de dificuldade dos desafios Leviatã? É muito estranho, vamos fazer neste desafio exatamente o que fizemos antes. 

## Steps

Após conectarmos com o usuário leviathan3, podemos ver que existe um arquivo no diretório home chamado "level3". Este é um arquivo executável. Assim como antes, vamos verificar o que ele faz ao executar este aplicativo.

```
Enter the password> test
bzzzzzzzzzzap. WRONG
```

O programa pede uma senha. Eu escrevi um "test" simples para ver o que acontecia, e ele retornou o texto "incorreto". Portanto, precisamos encontrar a senha correta do programa para descobrir a senha do próximo level.

Usando o comando `strings` para verificar se há alguma string secreta, podemos ver que existem muitas chamadas de bibliotecas. Mas nenhuma senha interessante em potencial. Então vamos usar o comando `ltrace` como antes:

```shell
ltrace ~/level3
```

Usando o ltrace, podemos ver que ele compara algumas strings usando a função `strcmp()`. Uma delas é a string "h0no33" com "kakaka", que não temos como alterar, então vamos ignorá-la por enquanto. Em seguida, ele pede uma senha. Digitamos "test" novamente para ver o que acontece e outra comparação aparece como resultado:

```
strcmp("test\n", "snlprintf\n")
```

Nesse caso, o caractere "\n" significa uma nova linha, ou seja, um caractere que indica que pressionamos Enter. Removendo o caractere de nova linha, estamos comparando a string "test" com "snlprintf", o que significa que a senha é "snlprintf".

Se digitarmos "snlprintf" no programa `level3`, seremos redirecionados para um terminal bash com o usuário leviathan4. Podemos verificar isso usando o comando `whoami`:

```shell
$ whoami
leviathan4
```

Agora, basta lermos a senha em `/etc/leviathan_pass/leviathan4` para obter a senha do próximo level.

