---
title: "Level 26"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Bandit - Level 26

## Objetivo
> Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
> 
>    NOTE: if you’re a Windows user and typically use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

Em uma tradução livre, seria algo como:

> Fazer login no usuário bandit26 a partir do bandit25 deve ser relativamente fácil… O shell do usuário bandit26 não é /bin/bash, mas sim outro. Descubra qual é, como funciona e como escapar dele.
> 
>    NOTA: Se você usa o Windows e costuma usar o PowerShell para acessar o Bandit via SSH: o PowerShell pode causar problemas com a solução proposta para este nível. Recomenda-se usar o prompt de comando.

## Comandos que você pode precisar para resolver esse level
> ssh, cat, more, vi, ls, id, pwd

## Passos

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


