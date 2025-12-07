---
title: "Level 00"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan - Level 00

Olá e sejam bem-vindes à minha jornada no Over The Wire, um dos meus CTFs favoritos. Agora, vamos encarar a seção Leviathan, dedicada aos primeiros passos da engenharia reversa.

Lembre-se que esta documentação é apenas para mim. Se você não resolveu este CTF sozinho, não leia estas postagens. Volte e resolva os desafios por conta própria. Não estrague sua experiência.

## Passos

Este é o primeiro desafio, é muito simples e depois de fazer a seção Bandit pode parecer óbvio. Primeiro, nos conectamos ao SSH do overthewire com o usuário leviathan0. Aqui estão as credenciais para o primeiro desafio:

```
Usuário: leviathan0
Senha: leviathan0
```

Para acessar via SSH, será algo assim:

```shell
ssh leviathan.labs.overthewire.org -p 2223 -l leviathan0
```

Ao ser solicitada a senha, digite `leviathan0`. Agora você está conectado, entrando diretamente no diretório inicial home `~`.

Usando o comando `ls -la`, podemos encontrar uma pasta oculta chamada ".backup", provavelmente de um backup do sistema. Dentro dela, temos um arquivo HTML chamado "bookmarks.html", provavelmente um backup de favoritos de um navegador antigo. Então, vamos verificar o conteúdo com o comando `cat`.

```shell
cd .backup
cat bookmarks.html
```

Em seguida, é exibido muitas e muitas linhas de texto, o que é normal para um arquivo de backup, e no cabeçalho do arquivo podemos encontrar uma referência ao Netscape, um navegador mais antigo.

Mesmo com tantas linhas de texto, precisamos focar e encontrar o que procuramos: a senha para o próximo level. Então, vamos usar o comando `grep` para filtrar algumas palavras. Talvez assim encontraremos o que procuramos.

```shell
cat bookmarks.html | grep "pass"
```

Ao pesquisar pela palavra "pass" (como em "password"), temos um resultado do arquivo, um "Fix later" que contém a senha para o próximo level.
