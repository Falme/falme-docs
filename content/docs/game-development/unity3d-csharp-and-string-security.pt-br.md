---
title: "Unity3D, C# e Segurança de Strings"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Unity3D, C# e Segurança de Strings

Originalmente esse documento era uma apresentação para uma empresa em que eu trabalhava, mas que nunca tive a chance de apresentar para meus colegas que trabalhavam com a Unity3D (Por ser ~Muito Perigoso~..., não é perigoso).

O nome original da apresentação era **Introdução à Segurança da Informação (e por que não colocar senhas dentro do código?)**. Espero que curtam.

## Do que iremos falar?

Eu sou um desenvolvedor de jogos digitais, e em todas as empresa que eu trabalhei é utilizado a Unity3D/C# como Engine principal, então vários exemplos apresentados serão voltados a essa ferramenta.

![Image description](/images/unity3d-csharp-and-string-security/1.jpg)

Nós iremos falar de:

1. O que é CyberSegurança? (rapidinho)
2. O que é Compilação e Decompilação
3. Um simples decompilação e leitura de senhas
4. O que é o Dotpeek e como usá-lo com a Unity
5. Unity com IL2CPP (e PlayMaker)
6. Inevitabilidade e Git
7. Conclusão
8. Referências e outros Links

Além disso, esse texto é somente para **AUTO DEFESA**, muitos/vários/quase todos os jogos e softwares tem um documento que precisa ser aceito dizendo que você não deve abrir o código do jogo (e é o que faremos hoje).

Tem várias empresas que lançam um jogo e no dia seguinte vários assets que seriam só revelados no Dia das Bruxas ou Natal agora estão por toda a internet, fazendo disso um inferno para o marketing do projeto que estava preparando para uma surpresa, mas mais perigoso que isso, pode ter uma chave de API disponivel para os jogadores poderem acessar Rankings de um jogo competitivo, dinheiro dentro do jogo ou algo similar.

Nós estamos aqui para ajudar você a prevenir (mas ainda possível) hackers de acessarem o que eles não devem. No caso desse documento, as strings/chaves/segredos dentro do seu jogo.

Mas primeiro, acho que você é "Só" um desenvolvedor de jogos, então vamos falar de cybersegurança.

---

## O que é CyberSegurança?

A definição de CyberSegurança segue:

"Proteção da informação e sistemas de informação de acessos não autorizados, o uso, vazamento, pertubação, modificação ou destruição a fim de providenciar sua confidencialidade, integridade e disponibilidade."

Tem muitos termos nesse parágrafo, mas nós iremos focar em:

**Proteção da informação e sistemas de informação de acessos não autorizados** : Onde a informação de um jogo é o valor que não deve ser acessado pelos jogadores.

E para o esclarecimento de alguns termos para esse documento: Confidencialidade, Integridade e Disponibilidade (Conhecido em inglês como C.I.A.)

Onde:

1. Confidencialidade : É basicamente dar acesso para aqueles que devem acessar aquela informação e impedir o acesso para aqueles que não devem acessar aquela informação


2. Integridade : Significa que a informação que aquele usuário receber é confiável e que ninguém modificou ela antes ou depois da transação. Isso garante que a informação de quem a recebeu é 100% idêntica a de quem enviou.

3. Disponibilidade : Isso garante que a informação precisa estar sempre disponível no espaço de tempo requerido, então, por exemplo, um site que acessa o seu jogo deve estar acessível, mas um hacker que faz um DDoS no seu site (fazendo seu site cair) não deveria acontecer baseado no princíprio da Disponibilidade. 

Mas a Disponibilidade não significa que a informação deve estar disponível o tempo todo pra sempre, somente precisa estar acessível no tempo que deve.

### Então como eram as proteções no passado?

Em torno dos anos 1960 só existiam senhas e proteções físicas, isso significa que o computador (a maioria só acessível nas Universidades) tinham uma sala com uma tranca que o protegia de ser acessado por quem não deveria.

Isso faz com que os ataques sejam na maioria físicos, sempre um acesso pessoal à máquina e não existia a ideia de algo como um Vírus. Mas não demorou muito para isso se tornar uma realidade.

Em torno dos anos 1970 começou o conceito de Antivírus e Proteção Virtual, muito porque a ARPANET tava ganhando força pelos EUA. Mas especialmente porcausa de um cara chamado Bob Thomas que criou o primeiro vírus conhecido: the Creeper.

![Image description](/images/unity3d-csharp-and-string-security/2.jpg)

Bob Thomas criou um programa que se auto-replica e se espalha pela rede de computadores, ele não fazia nada destrutivo, ele só desenhava um texto na tela escrito "I'M THE CREEPER: CATCH ME IF YOU CAN".

A consequência de sua criação faz com que Ray Tomlinson crie um novo programa que elimina o "The creeper", um programa chamado "Reaper".

Com isso, o primeiro vírus (The Creeper) e o primeiro Antivírus (Reaper) foram criados.

Ok, The Creeper era na verdade um Worm, ele se auto-propaga pela rede, mas esses nomes (Vírus, antivírus, worm) não era algo até 1980 onde os computadores pessoais e a internet como conhecemos era algo real.

Com o tempo vírus, worms, antivírus e similares começaram a se tornarem mais complexos. Hoje em dia, informação é mais preciosa que qualquer outra coisa, e precisamos protegê-lo da maneira correta.

### HACKERS!?

![Image description](/images/unity3d-csharp-and-string-security/3.jpg)

Hackers pela definição de Wesley Chai:

"Um hacker é um individual que usa a computação, redes e outras habilidades para sobrepor um problema técnico. O termo também se refere a qualquer um que use suas habilidades para ganhar acessos antes não permitidos em redes ou redes para cometer crimes. Um hacker pode, por exemplo, roubar informação para machucar pessoas via roubo de indentidade ou derrubar um sistema e, muitas vezes, manter como sequestro até que colete uma recompensa.

O termo Hacker historicamente tem sido bem divisivo, algumas vezes sendo usado como um termo de admiração por individuais que exibem um nível altissimo de habilidades e criatividades para resolver problemas técnicos. No entanto, o termo também é normalmente aplicado a individuais que usa essas habilidades para usos ilegais e não éticos."

Então, várias palavras, mas precisamos falar sobre algumas palavras usadas no parágrafo anterior:

Hacker, antes do termo cybersegurança ser aplicado, era utilizado para pessoas que são "Gambiarreiras" ou trazem soluções inteligentes para resolver um problema num sistema limitado.

Hackers cometerem crimes é uma frase desatualizada, hoje em dia nós devemos separar em chapéus:

**White Hat** (Chapéu Branco) : Hacker Ético, não comite crimes
**Black Hat** (Chapéu Preto) : Usa suas habilidades de hacker para ganhar acessos não autorizados. (normalmente cybercrime)
**Gray Hat** (Chapéu Cinza) : Usa suas habilidades para seu benefício próprio, talvez um trabalhador de TI de dia e um cybercriminoso durante a noite para seu próprio benefício.

Resumindo, hackers podem ser muito bons em encontrar falhas de segurança em sistemas como na empresa em que trabalha (com a permissão para testar o sistema, obviamente) a achar maneiras de fazê-lo mais seguro. Mas da mesma forma que pode ser positivo, também pode ser destrutivo com cybercrimes, roubo de informações, espiar usuários sem o conhecimento deles, etc.

---

## O que é Compilação e Decompilação?

Quando você, sim Você, o desenvolvedor de jogos, faz um código pra um jogo e precisa lançar ele para o público (fazendo um .exe, por exemplo) você precisa fazer uma compilação do seu código. Para simplificar, nós iremos definir como:

**Compilação**:
Código C# → Para → Código de Máquina

O código de máquina não é legível para desenvolvedores/humanos. Mas se nós só tivermos o código compilado, e por algum motivo precisarmos lê-lo, o processo de reversão é chamado de **Decompilação**:

**Decompilação**
Código de Máquina → Para → Código C# (Legivel para humanos)

Nesse caso, vamos pegar um código legível em C# vindo do programa original. Normalmente uma Decompilação deve ter a possibilidade de recompilar o código para um executável novamente. Esse é o objetivo final.

---

## Uma decompilação simples e leitura de senhas

![Image description](/images/unity3d-csharp-and-string-security/4.jpg)

Quando você passa pelo processo de decompilação, talvez o código resultante não seja o mesmo que o original, mas com certeza age igual ao original. Isso porque no processo de compilação o código (C#) precisa ser convertido em outra linguagem para ser processado em linguagem de máquina, algumas (maioria) das tecnicalidades do C# não estarão presentes e serão adaptadas.

No exemplo abaixo, nós temos um código original que verifica se a senha em uma variável é igual à "S3Cr37"

```csharp
void Start()
{
   if(pass == "S3Cr37")
   {
      Debug.Log("you have access");
   }
}
```
e depois de passarmos esse código pelo processo de decompilação, o código se parece com:

```csharp
void Start()
{
   if(!(this.pass == "S3Cr37"))
      return;

   Debug.Log((object) "you have access");
}
```
Meio diferente, mas eles fazem a mesma coisa, o código de baixo nível funciona meio "Pulando de uma linha para outra", então o codigo decompilado prefere sair do métido numa falha de verificação (isso explica a chamada do "return;")

Mas não devemos perder o foco, nossa "Senha Secreta" ainda está visível, esse string poderia ser nossa senha secreta, nossa chave de API secreta ou um endereço para uma página.

Então, antes de entender como prevenir, vamos ver como que isso funciona.

---

## O que é o Dotpeek e como usá-lo com a Unity

Nós estaremos usando a Unity como um exemplo para esse exercício, mas se precisar, eu subi o [projeto exemplo e o código fonte](https://github.com/Falme/Unity3D-String-Hack) no GitHub (Mas é tão simples que você pode fazer por conta própria).

Nós vamos criar um projeto Unity com as configurações padrão, adicionar numa cena um script chamado "Secret.cs" com o nosso código secreto. Só isso e fazer uma build para Windows (Desculpe usuários de linux D:, talvez o processo seja parecido de qualquer forma)

[Aqui está nossa Build](https://github.com/Falme/Unity3D-String-Hack/releases/tag/Build), vamos quebrar esse Secret.cs;

**Mas Ei, isso só acontece com amadores, certo?**
- De maneira alguma, vários jogos grandes que você conhece tem essa tecnicalidade que faz com que seja possível ler o código do jogo, aqui estão alguns exemplos (Não faça nada com esses códigos):

![Image description](/images/unity3d-csharp-and-string-security/5.jpg)

(Tome nota sobre INSIDE, que tem um "+/-", esse é um caso especial, nós iremos falar sobre isso mais pra frente)

O real perigo está no acesso do público/players ao conteúdo secreto dentro do jogo.

Com a build em mãos, agora nós precisados baixar uma ferramenta chamada [DotPeek](https://www.jetbrains.com/pt-br/decompiler/) da JetBrains. A ferramenta é um decompilador, para que possamos desmontar as .DLLs da build.

### Pequena parada para explicar como as DLLs funcionam com a Unity

![Image description](/images/unity3d-csharp-and-string-security/6.jpg)

Por padrão todos os scripts que você (desenvolvedor) cria na engine, será compilada em um Assembly Definition chamado **Assembly-CSharp.dll**, lembre-se desse nome, ele vai ser importante no futuro.

Isso faz com que o código da Unity seja estável e separado do código do desenvolvedor. Se você não quiser todo o seu código em uma DLL só, você vai precisar criar uma nova Assembly Definition no seu projeto e o separar numa pasta, mas sua referência AINDA aponta para o Assembly-CSharp.

Ok, fim do intervalo.

### Depois de terminar de baixar o DotPeek

![Image description](/images/unity3d-csharp-and-string-security/7.png)

Depois de baixar o Dotpeek, você vai precisar saber se a sua build alvo é vulnerável a esse ataque. Pra saber isso, você precisará achar a pasta da build e encontrar o DLL no caminho:

```
\CaminhoParaBuild\NomeDaBuild\BuildName_Data\Managed\Assembly-CSharp.dll
```

Se nesse caminho a DLL existir, seu ataque poderá ser feito. Isso acontece porque a Unity deixa como padrão a opção em Player Settings > Other Settings > Configuration > Scripting Backend como Mono.

![Image description](/images/unity3d-csharp-and-string-security/8.png)

isso faz com que a build seja muito mais rápida, mas sacrifica outras partes como Performance e Segurança.

Então, chega de falação, abram seu DotPeek, e selecione "File > Open" e selecione seu alvo **Assembly-CSharp.dll** (Eu disse que isso seria importante):

![Image description](/images/unity3d-csharp-and-string-security/9.png)

![Image description](/images/unity3d-csharp-and-string-security/10.png)

![Image description](/images/unity3d-csharp-and-string-security/11.png)

Depois de carregar o projeto, você vai selecionar o Assembly-CSharp, clique com botão direito do mouse e selecione "Export to Project...", selecione uma pasta e espere o decompilador terminar o trabalho.

![Image description](/images/unity3d-csharp-and-string-security/12.png)

Depois de exportar, você terá os arquivos C# do projeto original agora legível para o ser humano (meio diferente, mas legível).

![Image description](/images/unity3d-csharp-and-string-security/13.jpg)

Parabéns, você conseguiu a senha secreta!

Mas espera, como podemos prevenir isso?

---

## Unity com IL2CPP e PlayMaker

Uma maneira de deixar as coisas um pouco mais difícil para quem está abrindo o seu código é mudar a opção do **Script Backend** para **IL2CPP (Intermediate Language To C++)** em "Edit > Project Settings > Other Settings > Configuration"

![Image description](/images/unity3d-csharp-and-string-security/14.png)

O que ele faz é não colocar seu código diretamente em uma DLL, mas converte seu código C# em uma Linguagem Intermediária (Microsoft Intermediate Language) e depois em C++, para aí então compilar em código de máquina.

Fazendo esse processo, ele não irá criar um Assembly-CSharp.dll

![Image description](/images/unity3d-csharp-and-string-security/11.png)

### Nenhum problema com isso... certo?

Sim, tem um problema nisso, e é que a String ainda é uma String. Nós vamos achar nossa senha secreta novamente, mas dessa vez vamos usar uns comandos de Bash no Linux.

Com nossos novos arquivos compilados para a build, nós precisamos ir até o arquivo "global-metadata.dat" em

```
\CaminhoParaBuild\NomeDaBuild\BuildName_Data\il2cpp_data\Metadata\global-metadata.dat
```

Nós iremos usar o [WSL (Windows Subsystem for Linux)](https://apps.microsoft.com/store/detail/windows-subsystem-for-linux/9P9TQF7MRM4R), que é um Linux dentro do Windows 10/11, se você não tem ele, você pode baixá-lo na Microsoft Store, é de graça.

Com WSL, nós iremos usar esse comando no nosso arquivo global-metadata:

```bash
strings global-metadata.dat | grep S3Cr37
```

Onde:
- `strings` lista todas as strings dentro de um arquivo
- `grep` Destaca/Mostra somente se o argumento foi encontrado
- `S3Cr37` é o argumento que queremos encontrar

Com isso, nós podemos ver que nossa senha secreta ainda pode ser encontrado na build.

![Image description](/images/unity3d-csharp-and-string-security/15.jpg)

Bem, claro que nós sabíamos antes qual que era o código que queriamos encontrar. Mas se nós soubessemos pelas builds anteriores quais palavras descartar ou se nós usássemos uma wordlist com valores relevantes (http, api, etc...), nós ainda encontraríamos resultados interessantes, incluíndo nosso segredo.

### Hora do PlayMaker

Primeiro, eu não estou dizendo que o PlayMaker é mais seguro, é só um fato curioso que faz sentido estar nesse documento.

[PlayMaker](https://assetstore.unity.com/packages/tools/visual-scripting/playmaker-368) é uma ferramenta de Script Visual, similar aos Blueprint da Unreal Engine, mas não oficial da Unity. O curioso caso é que o jogo INSIDE usa essa ferramenta, e isso faz com que o codigo do PlayMaker não possa ser convertido diretamente, mas ele é convertido em Assets.

![Image description](/images/unity3d-csharp-and-string-security/16.jpg)

Isso faz com que seja mais difícil de encontrar as informações no código com as técnicas anteriormente usadas.

A conversão do seu código para Asset pode também ser feitas de maneiras diferentes, como:

1. Usando Scriptable Objects: Onde você pôe a sua chave em um Asset
2. Usando PlayMaker ou Outro Visual Scripting : Mesmo que o exemplo acima, mas estes influenciam o modo de desenvolvimento do time
3. Usando Serviços Externos : Ainda usando Scriptable Objects, mas esse faz chamadas para serviços externos, mantendo seus dados/acessos fora do jogo (Nós iremos falar sobre isso mais tarde).

No caso do PlayMaker, o que isso faz é converter o seu Visual Scripting Data em um arquivo **resources.assets**. Então, se você decidir abrir o arquivo resources.assets com um comando `strings` no bash (como falamos anteriormente), você só irá receber baboseiras.

![Image description](/images/unity3d-csharp-and-string-security/17.jpg)

### Então, É possível de abrí-lo?

Sim... Mais ou Menos... você vai precisar de umas ferramentas mais especializadas para isso, uma que possa te mostrar os Assets dentro de uma Build.

Nós iremos usar uma ferramenta chamada [AssetStudio](https://github.com/Perfare/AssetStudio). Ela é de graça e pode ser acessada em seu repositório no Github.

Nesse examplo, nós iremos pegar um projeto com PlayMaker e abrir ele com AssetStudio:

![Image description](/images/unity3d-csharp-and-string-security/18.jpg)

Como mostrado na imagem acima, nós podemos encontrar, abrir e exportar os arquivos PlayMakerAssemblies e PlayMakerFSM para nosso sistema e talvez achar algo útil.

Eu irei parar por aqui sobre o PlayMaker, mas se você quiser saber mais, brinque com ele, faça seu exemplo em um projeto e tente achar algum conteúdo secreto.

Bonus: AssetStudio também é usado para encontrar imagens, sons, músicas, modelos 3D, texturas e qualquer outra coisa que a Unity categorize como um Asset, que pode ser exportado e lido. É assim que muito modelos e informações são encontradas tão rapidamente depois do lançamento de um jogo.

---

## Inevitabilidade e Git

Então... O que aprendemos até agora?

- Todas as opções acima foram terríveis
- Nenhum código é 100% seguro
- O que nós estamos fazendo é só atrasar o hacker em achar a chave secreta.

Tudo isso pode ser resumido no "Teorema do Macaco Infinito" (Escondi a identidade do macaco por segurança):

![Image description](/images/unity3d-csharp-and-string-security/19.jpg)

O teorema diz que _um macaco digitando aleatoriamente em um teclado por um intervalo de tempo infinito irá quase certamente criar um texto qualquer escolhido, como por exemplo a obra completa de William Shakespeare._

No nosso caso, o que os hackers tem é tempo, então, com o tempo e paciência, o hacker vai encontrar a sua chave secreta (também vale lembrar que não é só um macaco, se o seu jogo for publicado na steam) 

### Então como hackers ainda conseguem achar minha chave secreta? 

Aqui estão alguns exemplos:

1. **Arquivos GitHub e Bitbucket**: Os hackers podem acessar/hackear os arquivos .git em algum momento e ir diretamente ao código fonte, e isso não é impossível, alguns jogos AAA tiveram esse incidente, o exemplo com o Git também tem acesso ao histórico de todos os seus commits, fazendo disso possível outros ataques (talvez?)

2. **Autenticação de Dois Fatores e Métodos de Login Obsoletos**: Alguns usuários não tem Autenticação de dois fatores ativo, no Github, na Unity, no Google Play Store, na Steam, e assim vai. Essa autenticação garante que é você mesmo que está acessando. [Aqui está um video engraçado do Game Newell pedindo para ser hackeado, mas a Autenticação de Dois Fatores bloqueia os acessos.](https://www.youtube.com/watch?v=gYs9nS8LlZ8)

3. **Low-Hanging Fruit**: Fazer o basico é essencial, mas os hackers amam ir atrás daqueles que nem o basico fazem, porque é fácil de hackear. Então se você não fizer o básico (como mudar para IL2CPP) eles vão usar uma automatização para te achar mais rápido.

4. **Strings e Códigos Encriptados**: Fazer uma encriptação é bom para todos, para mandar um email ou manter um dado secreto. Mas tenha certeza de não manter a chave ao lado da gaveta trancada. Tenha certeza que sua encriptação é boa, porque se for somente um ROT13, ele não vai fazer qualquer diferença.

### Outras maneiras de fazer ser mais seguro?

1. **Use Serviços Externos como [PlayFab](https://azure.microsoft.com/pt-br/products/playfab)**: PlayFab é um serviço onde você consegue colocar dados, rankings e codigo remoto, então se você precisar enviar uma pontuação para um jogo competitivo, você pode ter um código remoto que verifica se a pontuação está correta (ou viável) de ser adicionada. Mas mais que isso, verificar se uma transação monetária é legitima, ver os dados de um jogador e se necessário banir ele, etc. Dessa forma, sua chave/serviço não fica no seu jogo, então sua build está a salvo.

Esse conteúdo remoto também pode ser aplicado a conteúdos para eventos/feriados, onde você pode colocar um Asset Bundle (ou Adressables) em um serviço/servidor CDN e lançar quando o tempo chegar (evento de natal, por exemplo), então esse conteúdo e Assets não ficam na sua Build.

2. **Permissões para Leitura e Modificação**: Tenha certeza que alguns dados no seu servidor (enviando Requests para um servidor) não podem ser modificados se não devem ser modificados. É facil deixar isso passar, pois não pensamos nisso até o momento que alguém vai lá e remove/modifica os dados, mas tenha certeza que os acessos estão corretos.

3. **Faça o básico e um pouco mais**: Eu vejo outras áreas como Desenvolvimento de Software ou Desenvolvimento de Sites sendo bem cautelosos sobre Segurança da Informação (ok, não tanto assim, mas eles fazem o básico), e em desenvolvimento de jogos isso não é nem discutido. E as vezes a solução é tão simples. Então tome cuidado com seus projetos, não deixe nenhum valor secreto que não deve ser lido dentro das suas builds, especialmente se for um jogo online, verifique várias vezes se for necessário.

---

## Conclusão e Recomendações

Vamos recapitular:

1. Faça builds da Unity com o Script Backend: IL2CPP
2. Não use strings comprometedoras dentro de scripts
3. Use proteções recomendadas para Servidores Git (Github, Bitbucket, etc.) com [Chaves SSH RSA](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
4. Se você precisar registrar um valor, use serviços externos que tem uma camada de autenticação

### Outras Recomendações de Segurança fora da Área GameDev

1. Não abra links ou Emails suspeitos (nem de amigos)
2. Ative a [Autenticação de Dois Fatores](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication) em todos os serviços (Steam, Github, Google, etc...)
3. Use Antivírus (Melhor que nada)
4. Cheque se sua senha foi vazada (https://haveibeenpwned.com/)
5. Não use contas pessoais dentro da sua empresa
6. Mantenha todos os seus softwares atualizados (especialmente o Windows)
7. Bom senso

### Bônus: Isso pode ser útil no Desenvolvimento de Jogos?

Sim! Isso é uma técnica bem comum hoje em dia (Abrir builds e procurar por segredos), você, como um desenvolvedor de jogos, pode usar isso para adicionar segredos nos seus jogos, ou fazer um jogo em que PRECISA abrir o jogo para encontrar pistas. Mas pense nisso como um conteúdo que não é necessário para a Gameplay, só uma piada ou extra para os fãs hardcore.

É possível, e você pode fazer isso também.

---

## Referências (E outros links)

History of CyberSecurity, Avast : https://blog.avast.com/history-of-cybersecurity-avast#the-1950s

A History of Information Security :
https://www.ifsecglobal.com/cyber-security/a-history-of-information-security/

Unity Assembly Definitions : https://docs.unity3d.com/Manual/ScriptCompilationAssemblyDefinitionFiles.html

Dotpeek from Jetbrains : https://www.jetbrains.com/pt-br/decompiler/

The amazing history of programming with Olga Stern : https://www.youtube.com/watch?v=bJWWbql0QIQ

Tutorial Inspecting Unity Exported Assets
https://www.vg-resource.com/thread-31141.html

Jacquard's Loom machine:
https://www.youtube.com/watch?v=MQzpLLhN0fY

How to Be a Hacker:
http://www.catb.org/~esr/faqs/hacker-howto.html

Search Security - What is a Hacker? :
https://searchsecurity.techtarget.com/definition/hacker

Unity AssetStudio :
https://github.com/Perfare/AssetStudio/releases

CyberSecurity vs Information Security:
https://www.simplilearn.com/information-security-vs-cyber-security-article


![Image description](/images/unity3d-csharp-and-string-security/20.jpg)

