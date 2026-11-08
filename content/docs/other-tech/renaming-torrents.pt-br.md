---
title: "Como renomear arquivos de torrent"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
## Como renomear e mover arquivos baixados por torrent?

### O problema? Falta de seeds!

Sim, temos um problema com torrents atualmente, em parte devido ao streaming e aos downloads diretos das páginas em alta velocidade. Mas outra parte do problema é que a nova geração não sabe como usar torrents. Não explicamos como e por que usamos torrents para baixar arquivos da internet.

- Parece uma conversa sobre pirataria, mas na verdade não é. 
- Esta conversa é sobre a tecnologia Peer-2-Peer. 
- Esta conversa é sobre o número de um dígito de seeders na internet. 
- Esta conversa é sobre a preservação e a importância de termos uma cópia de mídias em cada um de nossos computadores.

Este texto explica como preservar arquivos na internet e manter os nomes corretos em uma pasta organizada em um servidor ou em seu computador pessoal.

Hoje vamos aprender como baixar um arquivo de um torrent, renomeá-lo, movê-lo para outro diretório e mantê-lo seedando, para que o próximo usuário possa baixá-lo em alta velocidade!


---

### Em primeiro lugar, esqueça o streaming!

Há uma grande vantagem em assistir a um filme, ler um livro, jogar online ou algo semelhante em qualquer lugar: É muito prático. Isso mesmo, hoje em dia temos internet de alta velocidade que torna isso possível, bem diferente da conexão discada de antigamente!

Mas precisamos pensar nisso de forma mais crítica. Na verdade, não somos donos desses arquivos, certo? O que acontece é o seguinte: baixamos uma parte do arquivo, essa parte é exibida na tela, depois baixamos a próxima parte e apagamos a anterior. Existe um fluxo em que o arquivo não é baixado por completo, apenas em pequenos fragmentos, enquanto consumimos o conteúdo.

Por exemplo, imagine que você está lendo um livro em um serviço de streaming. Esse livro baixa apenas as 5 páginas anteriores e as 5 páginas seguintes a partir da página que você está lendo. Todas as páginas seguintes são apagadas do seu computador. Isso não ocupa muito espaço, mas certamente você não terá o livro completo. Se a internet cair, você só terá mais 5 páginas para ler.

Agora você precisa de outro hobby...

Quanto mais pessoas usam streaming para consumir mídia ou baixar arquivos, mais difícil fica manter esses arquivos ativos na internet. Uma solução? Esqueça o streaming, abrace o torrent!

---

### Possuir, sempre!

Se você possúi a mídia ou os arquivos, existe a possibilidade de que esses arquivos sejam preservados. Afinal, eles estão com alguém, certo? E não com uma grande corporação. Os arquivos estão no seu computador, no do seu vizinho e também em um computador do outro lado do planeta.

Isso acontece porque você **possui** os seus arquivos e a mídia que está consumindo.

Vamos voltar ao exemplo do livro. Imagine que você baixou um livro por torrent. Agora ele é um arquivo no seu computador. Mas, por algum motivo, a empresa que detém os direitos autorais do livro simplesmente disse: "Ok, este livro não está mais disponível, pelo motivo X". Poxa, isso é um problema. Mas tudo bem, você ainda tem o livro, você já **possui** a mídia. E se você continuar compartilhando o livro, ele ficará disponível para outras pessoas também, facilitando o download e o upload dos dados na internet.

Este é um exemplo radical, mas pode se referir a qualquer coisa. Uma distribuição Linux específica, um jogo feito por fãs em 1999 para Master System, etc. O importante aqui são as possibilidades tecnológicas, e não como chegamos a elas.

Você deve possuir o que está consumindo.

---

### Exemplo: Linux OS


> **AVISO:** Vamos estabelecer uma conexão peer-to-peer. Por motivos de segurança, recomendo que você use uma VPN para ocultar seu endereço IP. Devido a essa conexão, quem se conectar com você poderá ver seu endereço IP. Por favor, navegue com segurança. 


Chega de conversa, vamos baixar a distribuição Linux: Ubuntu!

Esta é uma distribuição Linux gratuita e de código aberto baseada no Debian. Este é apenas um exemplo. Você pode fazer isso com QUALQUER arquivo torrent.

Vamos acessar [https://ubuntu.com/download/alternative-downloads](https://ubuntu.com/download/alternative-downloads) e baixar o arquivo torrent mais recente do Ubuntu Desktop LTS (para mim, a versão atual era "Ubuntu Desktop 26.04 LTS (64 bits)") na aba BitTorrent.

Isso fará o download de um arquivo com um nome semelhante a "ubuntu-{versão}-desktop-amd64.iso.torrent". Com isso certo podemos continuar, mas primeiro vamos baixar um cliente de torrent para baixar o Ubuntu.

---

### Baixando o qbittorrent

O cliente de torrent escolhido para este exemplo é o qBittorrent. Ele foi escolhido por ser gratuito e de código aberto, mas o processo pode ser feito com qualquer cliente de torrent de sua preferência.

Então, primeiro, vamos baixar o cliente qBittorrent para o seu sistema operacional em: [https://www.qbittorrent.org/download](https://www.qbittorrent.org/download)
Após baixar e instalar o qBittorrent, abra-o e você verá uma fila de downloads vazia. Vamos mudar isso, clique no símbolo de mais (+) no canto superior esquerdo do programa para selecionar um arquivo torrent.

Selecione o arquivo torrent que baixamos do site do Ubuntu.

Após selecionar o torrent, será perguntado onde você prefere salvar o arquivo. Qualquer lugar serve, vamos realocá-lo de qualquer forma. Clique em "OK" e aguarde a conclusão do download.

---

### Renomeando o arquivo ISO

Imagine que precisamos renomear o arquivo que baixamos por torrent. Se alterarmos o nome no arquivo do sistema, perderemos a referência do torrent. Portanto, precisamos renomear o arquivo pelo aplicativo qBittorrent.

Primeiro, selecione o arquivo na fila de downloads do qBittorrent e, em seguida, selecione a opção "Content" na parte inferior do aplicativo. Isso mostrará os arquivos conectados à transferência do qBittorrent, como mostrado na imagem:

![Showing Content](/images/renaming-torrents/1.jpeg)

Esse arquivo é um espelhamento do arquivo existente no seu sistema. Portanto, vamos renomeá-lo. Clique com o botão direito do mouse no nome do arquivo e selecione "Rename":

![Renaming File](/images/renaming-torrents/2.jpeg)

Agora, você pode renomear como quiser. No meu caso, eu mudei para "Ubuntu-001.iso". Então, depois dessa alteração no qBittorrent, o arquivo no sistema também será modificado. Verifique a pasta de downloads e você verá que o nome também foi alterado.

---

### Movendo o arquivo ISO

Ok, imagine que você comprou um novo HD ou SSD e precisa mover este arquivo ISO para um novo local. Como faço isso sem perder a referência de transferência? Movemos os arquivos pelo qBittorrent, novamente.

Da última vez, renomeamos usando a aba "Content", mas desta vez selecionaremos o download na fila, clicaremos com o botão direito do mouse e selecionaremos "Set location...", como na imagem a seguir:

![Moving File](/images/renaming-torrents/3.jpeg)

Isso abrirá uma janela para selecionar o novo local do conteúdo. Depois de selecionar o novo local, o qBittorrent moverá automaticamente os arquivos para o novo local e sincronizará novamente para verificar se tudo está correto.

Dessa forma, podemos mover o arquivo e mantê-lo disponível para outros usuários.

---

### Conclusão

#### Por que usamos torrent?

Além da preservação, há também o benefício da alta velocidade de download. Sério! Se você nunca usou torrent para baixar arquivos, pode se surpreender com a velocidade que ele pode alcançar. Com o número certo de seeders, você pode ter arquivos pesados em um instante!


#### Preservação novamente

Repito, preciso dizer: não podemos deixar que o streaming crie uma nova onda de Lost Media (Mídias perdidas)! Use torrents, possua seus arquivos, distribua-os pela internet!


#### Comunidade na web

Além da preservação, você pode encontrar alguns geeks e nerds com gostos parecidos com os seus. Talvez você encontre livros que contam histórias dos anos 1600, ou descubra distribuições Linux muito, muito estranhas, mas muito legais.

É possível encontrar uma comunidade na internet, explore, esteja presente, descubra coisas bizarras! (segurança em primeiro lugar, lembre-se, oculte seu IP)
