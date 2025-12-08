---
title: "Level 02"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan - Level 02

Este desafio nos ensina sobre os perigos de redirecionar a execução de um programa de um usuário para outro, neste caso, o comando `cat`.

## Passos

Após conectar com o usuário leviathan2 usando `ssh`, chegamos ao diretório home com um arquivo executável chamado "printfile". Ao executar o arquivo, percebemos que ele requer um argumento, um arquivo a ser lido. Assim, ao passar qualquer arquivo como argumento, o comportamento é semelhante ao do comando `cat`.

Vamos verificar alguns detalhes usando o comando `strings` para examinar qualquer string que nos seja útil:

```shell
strings ~/printfile
```

E a saída nos mostra que ele não se comporta de forma semelhante ao comando `cat`. Ele **é** o comando `cat` como um redirecionamento:

```
Usage: %s filename
You cant have that file ...
/bin/cat %s
```

Isso nos dá algumas ideias, uma delas é injetar nosso código onde o argumento de string (%s) é esperado, fazendo com que ele execute dois comandos em um. Então, vamos criar alguns arquivos em um diretório temporário usando `mktemp -d` e entrando no diretório criado. Agora podemos criar um arquivo teste e dar as permissões corretas ao usuário leviathan3.

```shell
touch "test"
chmod 777 "test"
~/printfile "./test"
```

Se as permissões estiverem disponíveis para o usuário "leviathan3", podemos ver que nenhum erro é retornado. Portanto, vamos combinar dois comandos em um só usando o nome do arquivo, fazendo com que o programa interprete o nome do arquivo como um comando. Primeiro, vamos testar com o comando `whoami` para verificar se tudo está funcionando como esperado:

```shell
mv "test" "test && whoami"
~/printfile "./test && whoami"
```

Isso retorna o usuário "leviathan3", ótimo, estamos executando alguns comandos como o usuário leviathan3. Agora, vamos tentar acessar o bash do leviathan3 usando o comando `bash`:

```shell
mv "test && whoami" "test && bash"
~/printfile "./test && bash"
```

Agora estamos no terminal do usuário leviathan3. Basta acessar o arquivo `/etc/leviathan_pass/leviathan3` para obter a senha do próximo level.

