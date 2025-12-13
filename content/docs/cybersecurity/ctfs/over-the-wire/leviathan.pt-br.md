---
title: "Leviathan"
# weight: 0
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Over The Wire - Leviathan

Olá e sejam bem-vindes à minha jornada no Over The Wire, um dos meus CTFs favoritos. Agora, vamos encarar a seção Leviathan, dedicada aos primeiros passos da engenharia reversa.

Lembre-se que esta documentação é apenas para mim. Se você não resolveu este CTF sozinho, não leia estas postagens. Volte e resolva os desafios por conta própria. Não estrague sua experiência.

---

## Level 0

### Passos

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

---

## Level 00 -> Level 01

Este desafio tem 2 maneiras de ser resolvido: uma aleatória (a que eu resolvi primeiro) e outra não aleatória. Vamos conferir ambas.

### Passos

Após conectar com o usuário leviathan1 usando `ssh`, temos o diretório home com um arquivo executável chamado "check". Então, primeiro executamos para ver o que ele faz:

```shell
./check
```

Este programa pede uma senha e, se estiver incorreta, retorna uma mensagem de "adeus", encerrando o programa. Se a senha estiver correta, retorna algo que ainda não sabemos. Então, vamos ver como podemos resolver isso.

#### Um Jeito Aleatório

Usando o comando `strings` para exibir todas as strings do programa, podemos ver que nada nos chama a atenção. Algumas palavras possíveis aqui e ali, mas nada promissor.

Então, podemos usar o comando `xxd` para exibir um hexdump com os valores lado a lado. E uma coisa que me chamou a atenção é que algumas palavras aparecem no hexdump, mas não no comando `strings`, uma delas é a palavra "sex". Ok, estranho, mas não custa tentar. Então, executando `./check` e digitando a palavra "sex", de fato, retornamos a um shell. A senha está correta.

#### Um Jeito Não Aleatório

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

---

## Level 01 -> Level 02

Este desafio nos ensina sobre os perigos de redirecionar a execução de um programa de um usuário para outro, neste caso, o comando `cat`.

### Passos

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

---

## Level 02 -> Level 03

Já falei com vocês sobre a progressão de dificuldade dos desafios Leviatã? É muito estranho, vamos fazer neste desafio exatamente o que fizemos antes. 

### Steps

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

---

## Level 03 -> Level 04

### Passos

Após conectar ao leviathan4, podemos ver no conteúdo do diretório home com `ls -la` que temos um diretório oculto chamado ".trash" e dentro dele há um arquivo executável chamado "bin".

Ao executar, o arquivo "bin" retorna um texto em binário. 

Este é muito simples, basta inserir o código binário em algum "conversor de binário para texto" online ou usando o CyberChef, e a senha para o próximo level será revelada. 

---

## Level 04 -> Level 05

