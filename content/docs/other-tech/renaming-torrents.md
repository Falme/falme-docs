---
title: "How to rename torrent files"
# weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
## How to rename and move downloaded torrent files?

### The Problem? No Seeds!

Yes, we have a problem with torrent nowadays, and partially is because of streaming and high-speed direct downloads. But other part of the problem is that the new generation doesn't know how to use torrent, we have not explained how and why we use torrent to download things on the internet.

- This looks like a piracy talk, but actually it's not. 
- This talk is about the Peer-2-Peer technology. 
- This talk is about the single digit number of seeders in the internet. 
- This talk is about preservation and having a copy of a media in each one of our computers.

This text is about how you make a preservation of files through the internet and keep the names correct in a organized folder in a server or your own personal computer.

Today we will learn how to download a file from a torrent, rename it, move it to another directory and keep it seeding, for the next user download this file in a high speed!

---

### First of all, forget Streaming!

There's a benefit on watching a movie, reading a book, streaming a game, or something similar, on the fly, it's very practical. That's right, nowadays we have a high-speed internet that make this possible, different from the dial-up internet connection from the past!

But we must think more critically about it. We actually do not own this files, right? What happens is: we download a chunk of the file, this chunk will be shown at the screen, then it will download the next chunk and delete the last chunk. There's a flow where it doesn't download all the bytes of the file, just little pieces while you consume the media.

As an example, imagine we are reading a book on the fly from streaming service. This book will only download 5 pages before and 5 pages after the currently page you are reading. Every page besides that will be deleted from your machine. This will not occupy much space on the machine, but you surely do not have the whole book. If the internet is cut, you only have more 5 pages to read.

Now you need another hobby...

The more people using streaming to consume media or download files, the harder is to keep those file active on the internet. A solution? Forget streaming, embrace torrenting!

---

### Owning, always!

If you own the media or files, there's a chance that these files will be preserved. Because it is with someone, right? And it's not with a big corporation, it's on your machine, and your neighbor machine, and in a machine at the other side of the planet as well.

This happens because you **own** your files and the media you are consuming.

Let's go back to the book example. Imagine that you download a book from a torrent. Now it's a file in your computer. But for some reason, the company that owns the rights to the book just said "Ok, this book is no longer available, because X reason". Oh damn, this is a problem. But it's ok, you still have the book, you already **own** the media. And if you are still seeding the book, this will be available to other people to seed this file as well, making it easy to download and upload it's data in the internet.

This is a radical example, but it can be about everything. A specific Linux distribution, a fan game made in 1999 for Master System, etc etc. The point here is the technology possibilities, not how we get here.

You must own what you are consuming.

---

### Example: Linux OS


> **WARNING:** We are going to make peer-2-peer connection, for security reasons, I recommend you use a VPN to hide your IP. Because of the connection, those who make a connection with you can and will see your IP. Please be safe online. 


Too much talk, let's download a Linux distribution: Ubuntu!

This is a free and open source Linux distribution based on Debian. This is just an example, you can do it with ANY torrent file.

We will go to [https://ubuntu.com/download/alternative-downloads](https://ubuntu.com/download/alternative-downloads) and download the latest Ubuntu Desktop LTS torrent file (for me, the current version was "Ubuntu Desktop 26.04 LTS (64-bit)") at the BitTorrent tab.

This will download a file with a name similar to "ubuntu-{version}-desktop-amd64.iso.torrent". This is correct, but first we will download a torrent client, to download Ubuntu.

---

### Downloading qbittorrent

The torrent client chosen for this example is qBittorrent. This is chosen because it's free and open source, but can be done with any torrent client of your choice.

So, first, let's download the qbittorrent client for your Operational System at: [https://www.qbittorrent.org/download](https://www.qbittorrent.org/download)

After downloading and installing qbittorrent, open it and you will be presented with an empty queue of downloads. Let's change that clicking at the Plus (+) symbol at the top-left of the program to select a torrent file.

Select the torrent file we downloaded from the Ubuntu website.

After selecting the torrent, it will ask where do you prefer to save the file, any place will do, we are going to relocate anyway, click "Ok" and wait to finish downloading.

---

### Renaming the ISO file

Imagine that we need to rename the file we downloaded from torrent, if we change the name in the system file, it will lost the reference. So, we must rename from the qbittorrent application.

First, select the file in qbittorrent downloading queue and then, select the option "Content" at the bottom of the application, this will show the files connected to the qbittorrent transferring, like this:

![Showing Content](/images/renaming-torrents/1.jpeg)

This file mirrors the file in your system. So, let's change it's name. Right click on the file name and select "Rename":

![Renaming File](/images/renaming-torrents/2.jpeg)

Now, you can rename the way you want. In my case, I changed to "Ubuntu-001.iso". So, after that change in the qbittorrent, the file in the system will also be modified. Go check in the downloads location, you'll see that the name is also modified.

---

### Moving the ISO file

Ok, imagine that you got a new HDD or SSD and need to move this ISO file to a new location. How do I do that without losing the transfer reference? We move the files through qbittorrent, again.

Last time, we renamed going through the "Content" tab, but this time we will select the download in queue, click with the right mouse button and select "Set Location..." just like in the following image:

![Moving File](/images/renaming-torrents/3.jpeg)

This will open a window to select the new location of the content. After you select the new location, qbittorrent will automatically move the files to a new location and re-sync to see if everything is correct.

This way we can move the file and keep it seeding for other users.

---

### Conclusion

#### Why we use torrent?

For preservation, but there's also the benefit of fast downloading speed. Really! If you didn't use torrent to download files, you may be surprised about the download speed that it can reach. With the correct number of seeders, you can have heavy files in an instant!


#### Preservation again

Again, I must say it again. We must not let the streaming create a new wave of Lost Media! Use torrent, Own your media, Distribute through the network!


#### Community on the web

Besides the preservation, you may find some geeks and nerds that have the same taste as you. Maybe with this you can find books that tell stories from 1300, or discover Linux distributions that is very very weird, but very cool.

A community can be found in the internet, explore, be there, discover weird shit! (safety first, remember, hide your IP)
