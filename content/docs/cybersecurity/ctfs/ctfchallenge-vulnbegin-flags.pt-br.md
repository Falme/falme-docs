---
title: "CTFChallenge - VulnBegin Flags"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# CTFChallenge - VulnBegin Flags

Às vezes eu acho muito interessante as atividades de Capture the Flag (CTF), onde eu posso usar parte do meu tempo para mergulhar num problema que normalmente me pergunto "Como diabos eu vou resolver isso aqui?". E então, depois de algumas horas na cama, eu levanto dizendo "Já sei!" e tento minha nova ideia, às vezes funciona, às vezes não, mas de qualquer forma é bem satisfatório.

Esse aqui é sobre os desafios do site CTFChallenge, pra ser mais específico, o desafio VulnBegin Challenge, com 9 Flags pra serem encontradas.

No meu caso, o site vai ser relativo de tempos em tempos (toda vez é uma instancia nova do CTFchallenge), então, para esse documento, eu vou me referir a URL do desafio como "mymachine.vulnbegin.co.uk/".

Sem mais delongas, aqui está minha jornada para encontrar todas as flags.

---

## Flag 1

![Image description](/images/ctfchallenge-vulnbegin-flags/1.jpg)

Como está descrito no site, "Não tem muita coisa aqui" (tradução livre), nenhum botão pra clicar, nenhum formulário, sem flags dentro do Inspecionar elemento, então precisarei de meios alternativos pra achar a flag.

Normalmente eu começo olhando pelo robots.txt, que pode ter alguns endereços de arquivos e pastas, normalmente usado pra filtrar esses endereços para que não possa ser encontrado por automações (como a busca do Google) e manter esses endereços em segredo.

Então, eu dei uma olhada no endereço mymachine.vulnbegin.co.uk/robots.txt pra ver se encontro algum endereço escondido.

```
User-agent: *
Disallow: /secret_d1rect0y/
```

Com essa informação eu encontro uma pasta marcada como "Disallow" chamada /secret_d1rect0y/ . Indo nesse endereço eu encontro a primeira flag.

{{% details title="Answer:Flag 1" open=false %}}
```
[^FLAG^2B22E2CB70E218510802B0359488F6A2^FLAG^]
```
{{% /details %}}

---

## Flag 2

Essa flag foi meio problemática, eu entendi a ideia, mas logo eu explico minha dificuldade.

Dessa vez eu precisava achar um subdomínio do mymachine.vulnbegin.co.uk, mas o subdomínio não é fácil de acessar por bruteforce ou wordlists. 
Pra isso precisamos ir até o site `crt.sh` e procurar por certificados do `vulnbegin.co.uk`;

![Image description](/images/ctfchallenge-vulnbegin-flags/2.jpg)

Essa imagem mostra um problema, que o endereço no campo mostrado é `http://vulnbegin.co.uk/`, mas colocando o `http://` ou `/` no final do endereço, ele não vai encontrar os certificados que eu preciso, e eu gastei muito tempo com esse simples problema de string.

Depois de mudar o valor pra só `vulnbegin.co.uk`, eu tive os resultados corretos com o subdomínio que eu estava procurando:

![Image description](/images/ctfchallenge-vulnbegin-flags/3.jpg)

Com isso eu encontrei o subdomínio `v64hss83`, e acessando a página `v64hss83.mymachine.vulnbegin.co.uk` nós conseguimos acesso à flag:

{{% details title="Answer:Flag 2" open=false %}}
```
[^FLAG^047524FE61AE6B5FD1D184994C7322FC^FLAG^]
```
{{% /details %}}

---

## Flag 3

Encontrar os subdomínios de mymachine.vulnbegin.co.uk/ pode ser bem útil, e é recomendado usar ferramentas como FFuF pra enumerar os subdomínios, para isso, eu usei o comando

```bash
ffuf \
-w subdomains.txt \
-u http://FUZZ.mymachine.vulnbegin.co.uk
```

Onde:
- `ffuf` roda o programa 
- `-w subdomains.txt` usa uma wordlist pra escanear 
- `-u http://FUZZ.mymachine.vulnbegin.co.uk` define um endereço pra enumerar, onde FUZZ é a palavra que vai ser substituída e testar por possíveis valores encontrados.

Com esse comando, ele retornou um subdomínio possível chamado http://server.mymachine.vulnbegin.co.uk

![Image description](/images/ctfchallenge-vulnbegin-flags/4.jpg)

Checando o endereço, nós encontramos a flag:

{{% details title="Answer:Flag 3" open=false %}}
```
[^FLAG^E858ED9649E57BECE9ACD1A4C60D3446^FLAG^]
```
{{% /details %}}

---

## Flag 4

A página de Feramentas Úteis do CTFChallenge recomenda o uso de DNSRecon para verificar informações sobre o DNS de mymachine.vulnbegin.co.uk/ . Então eu rodei o comando 
```bash
dnsrecon \
-d mymachine.vulnbegin.co.uk \
-D ./subdomains.txt \
-t std --xml dnsrecon.xml \
--lifetime 5.0
```

Onde:
- `dnsrecon` roda o programa
- `-d mymachine.vulnbegin.co.uk` define o domínio pra verificar
- `-D ./subdomains.txt`  usa uma wordlist de subdomínios da página Ferramentas Úteis pra procurar por informações
- `-t std` chama um escaneamento padrão
- `--xml dnsrecon.xml` exporta os resultados para um arquivo XML chamado dnsrecon.xml 
- `--lifetime 5.0` mantém ativo o escanemento por até 5.0 segundos

Com o escaneamento, uma nova informação sobre o DNS é revelado e com ela, a flag

![Image description](/images/ctfchallenge-vulnbegin-flags/5.png)

{{% details title="Answer:Flag 4" open=false %}}
```
[^FLAG^BED649C4DB2DF265BD29419C13D82117^FLAG^]
```
{{% /details %}}

---

## Flag 5

Depois de rodar FFuF pra achar algumas páginas escondidas no endereço mymachine.vulnbegin.co.uk/* eu encontrei uma página chamada cpadmin:

![Image description](/images/ctfchallenge-vulnbegin-flags/6.jpg)

Dando uma olhada na página, nós encontramos um formulário de login:

![Image description](/images/ctfchallenge-vulnbegin-flags/7.png)

Então a primeira coisa a se fazer é o teste mais básico da tela de login: colocar admin/admin no usuário e senha. Pra minha surpresa o resultado me diz que a senha está incorreta, mas não o usuário, ou seja, existe um usuário chamado admin, eu só preciso encontrar a senha correta.

![Image description](/images/ctfchallenge-vulnbegin-flags/8.png)

Pra isso eu vou usar Hydra pra adivinhar a senha por força bruta. O comando 

```bash
hydra \
-l admin \
-P ~/wordlists/passwords.txt \
mymachine.vulnbegin.co.uk \
http-form-post "/cpadmin/login:username=^USER^&password=^PASS^:Password is invalid" -V
```
vai resolver isso.

Onde:
- `hydra` roda o programa para força bruta
- `-l admin` define o usuário para a string 'admin'
- `-P ~/wordlists/passwords.txt` chama uma wordlist de senhas para testar
- `http-form-post` é o method-form para Hydra atacar
- `/cpadmin/login:` é o caminho para a página de login
- `username=^USER^&password=^PASS^` é a informação sendo enviado para a requisição do login, onde ^USER^ vai ser substituído pelo valor do usuário e ^PASS^ será substituído pelo valor da senha
- `Password is invalid` é o identificador na página que verifica se o login teve sucesso ou não, nesse caso nós só estamos checando pela validação da senha, pois já sabemos o usuário
- `-V` verbose flag, assim podemos ver todas as tentativas da aplicação Hydra

Depois de algumas tentativas, nós encontramos a senha do admin:

![Image description](/images/ctfchallenge-vulnbegin-flags/9.jpg)

Após colocar a senha correta, somos apresentados com a flag:

![Image description](/images/ctfchallenge-vulnbegin-flags/10.jpg)

{{% details title="Answer:Flag 5" open=false %}}
```
[^FLAG^93D7491FB4B054FB5C5AC3E0292BE41C^FLAG^]
```
{{% /details %}}

---

## Flag 6

Depois de fazer login como Admin na página cplogin, ela diz que o arquivo de configuration estava disponível, então vou usar uma wordlist pra encontrar novos conteúdos usando os cookies da sessão pelo login.
Pra isso utilizaremos FFuF pra achar conteúdos com Cookies no Header:

```bash
ffuf \
-w ~/wordlists/content.txt \
-u http://mymachine.vulnbegin.co.uk/cpadmin/FUZZ \
-H "Cookie: token=2eff535bd75e77b6c70ba1e4dcb2873"
```

Onde:
- `ffuf` roda o programa
- `-w ~/wordlists/content.txt` usa uma wordlist chamada content.txt
- `http://mymachine.vulnbegin.co.uk/cpadmin/FUZZ` é o endereço onde quero encontrar novos conteúdos e a palavra FUZZ é a string a ser testada
- `-H "Cookie: token=2eff535bd75e77b6c70ba1e4dcb2873"` é o Cookie da sessão que queremos fazer com que a página acredite que estamos autenticados (na verdade esse cookie É o que verifica se estou ou não autenticado)

Após correr por toda a wordlist, encontramos algumas páginas:

![Image description](/images/ctfchallenge-vulnbegin-flags/11.jpg)

Vamos checar a página `env` :

![Image description](/images/ctfchallenge-vulnbegin-flags/12.png)

Com isso nós encontramos a flag e umas dicas para a próxima flag

{{% details title="Answer:Flag 6" open=false %}}
```
[^FLAG^F6A691584431F9F2C29A3A2DE85A2210^FLAG^]
```
{{% /details %}}

---

## Flag 7

O endereço `server.mymachine.vulnbegin.co.uk` retorna como "Não Autenticado" pelo login das ultimas flags, o motivo pode estar nesse novo parâmetro do Header `"X-Token: 492E64385D3779BC5F040E2B19D67742"`, talvez com ele poderemos nos autenticar no subdomínio `server`.

Então usando o comando 
```bash
curl http://server.mymachine.vulnbegin.co.uk/ \
-H "X-Token: 492E64385D3779BC5F040E2B19D67742"
```
e com o novo X-Token no Header, nós estamos autenticados. A nova flag aparece:

![Image description](/images/ctfchallenge-vulnbegin-flags/13.jpg)

{{% details title="Answer:Flag 7" open=false %}}
```
[^FLAG^0BDC60CC5E283476E7107C814C18DCCF^FLAG^]
```
{{% /details %}}

---

## Flag 8

Com o valor de autenticação do X-Token, nós podemos re-escanear por novos endereços, só que agora pelo subdomínio do server.
Então chamando o FFuF de novo pra achar novos conteúdos, só que agora com 2 valores no header, um sendo o X-Token pra autenticar o acesso `-H "X-Token: 492E64385D3779BC5F040E2B19D67742"` e o Host do servidor `-H "Host: server.mymachine.vulnbegin.co.uk"`.

```bash
ffuf http://mymachine.vulnbegin.co.uk/user \
-H "X-Token: 492E64385D3779BC5F040E2B19D67742" \
-H "Host: server.mymachine.vulnbegin.co.uk"
```
Com esses valores no Header, nós encontramos um novo acesso em /user/:

![Image description](/images/ctfchallenge-vulnbegin-flags/14.jpg)

Acessando a página /user/ nós encontramos um JSON com a seguinte informação:

`{"id":27,"endpoint":"/user/27"}`

E acessando a página /user/27/ nós encontramos outro JSON com a seguinte informação:

`{"id":27,"username":"vulnbegin_website","endpoint":"/user/27/info"}`

E acessando a página /user/27/info/ nós encontramos outro JSON com a flag

{{% details title="Answer:Flag 8" open=false %}}
```
[^FLAG^7B3A24F3368E71842ED7053CF1E51BB0^FLAG^]
```
{{% /details %}}

---

## Flag 9

Na última flag tinha uma ID em /user/27/info, o que significa que um usuário tem um identificador no banco de dados como 27. Isso revela que outros usuários podem estar disponível para ler suas informações.
A ideia é fazer um loop pelos usuário até achar algo interessante ou a própria flag.

Pra isso nós vamos usar um loop em bash/shell pra pegar essa informação pelo curl:

```bash
for i in {0..5}; \
do curl http://mymachine.vulnbegin.co.uk/user/$i/info \
-H "X-Token: 492E64385D3779BC5F040E2B19D67742" \
-H "Host: server.mymachine.vulnbegin.co.uk"; \
done
```

Onde:
- `for i in {0..5};` faz um loop pelas próximas 6 vezes (começa em zero, termina em 5)
- `do` define que ação fazer (próximo comando do loop)
- `curl http://mymachine.vulnbegin.co.uk/user/$i/info -H "X-Token: 492E64385D3779BC5F040E2B19D67742" -H "Host: server.mymachine.vulnbegin.co.uk";` O comando que vimos da última vez, mas agora com $i no lugar do ID do usuário, esse número vai mudar de acordo com o valor do loop (de 0 a 5)
- `done` identificador de fim do loop

Depois de fazer o loop 6 vezes, nós temos a flag na ID 5:

![Image description](/images/ctfchallenge-vulnbegin-flags/15.jpg)

{{% details title="Answer:Flag 9" open=false %}}
```
[^FLAG^3D82BE780F46EE86CE060D23E6E80639^FLAG^]
```
{{% /details %}}

---

## Conclusão

Essa foi uma jornada, muitas das soluções eu já vi antes, mas o CTF tem uma correta sequência de qual concluir e como achar a próxima flag, e a próxima flag, e a próxima... É motivante querer achar a próxima (Eu também achei mais 2 flags no desafio VulnLayers, o próximo CTF gratuíto, mas não irei terminar tão cedo).

O CTFChallenge me dá um sentimento semelhante aos jogos Escape the Room, onde eu encontro uma solução de um problema, mas ao mesmo tempo me dá uma dica do que eu deveria olhar depois.

Mas pelo lado negativo, se eu encontrar uma resposta do problema na ordem errada, vai ser bem confuso de terminar o CTF/Escape the room como um todo. No caso do CTFChallenge eu pulei a flag 2, e encontrei o resultado da flag 3, 4 e 5, sendo muito difícil de ter uma linha de raciocínio para seguir.

Como um aprendizado, eu preciso ter mais paciência e fazer meu caminho ser bem claro, o passo de cada vez até eu chegar no fim.
