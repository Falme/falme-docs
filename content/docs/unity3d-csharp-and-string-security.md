---
title: "Unity3d Csharp and String Security"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
# Unity3D, C# and String Security


This is originally a presentation to a company that I worked on but never had the chance to present to my co-workers (because it was ~Too Dangerous~..., it's not)

The original name of the presentation was **Introduction to CyberSecurity (And why never put passwords inside code)** as an introduction to security to the company and my co-workers.

## What Will We Talk About?

I am a Unity3D developer and all the companies that I've worked on uses Unity3D/C# as the main Engine, so many examples here will be oriented to that.

![Image description](/images/unity3d-csharp-and-string-security/1.jpg)

So We will talk about:

1. What Is CyberSecurity (quick one)
2. What is Compilation and Decompilation
3. A simple decompilation and password reading
4. What is Dotpeek and How to use with Unity
5. Unity with IL2CPP and PlayMaker
6. Inevitability and Git
7. Conclusion
8. References and Other Links

Also, this text is for **DEFENSE ONLY**, many/most/almost all games and software programs have a document that you sign to use that you should not open the code of the game (that's what we will do today). There are many companies that release a game and in the next day the secret assets for Halloween or Christmas is all over the internet, making a hell for the marketing of the project that was hiding it to be a surprise, but more dangerous than that, having the API key available for players that can access the Ranking of a competitive game, cash or similar.

We are here to help you to prevent (but still possible) hackers to access what they should not. In this document case, the strings/keys/words inside your game.

But first, I guess you are "just" a Game Developer, so let's talk about cybersecurity!

---

## What is CyberSecurity?

The definition of CyberSecurity reads as:

“Protection of information and information systems from unauthorized access, use, disclosure, disruption, modification, or destruction to provide confidentiality, integrity, and availability.”

There are many terms is this paragraph, but we will focus on:

**Protection of information and information systems from unauthorized access** : Where the information of a game is the data that shouldn't be accessed from players

And for knowledge of some aspects of this document: Confidentiality, Integrity and Availability (Most known as C.I.A.)

Where:

1. Confidentiality : Is basically giving access to those who should access that information and deny the access to those who shouldn't access that information.

2. Integrity : Means that the information that the user receive is reliable and no one modified it before or after the transaction. This guarantees that the information is 100% the same from the sender to the receiver.

3. Availability : That guarantee that the information need to be available at the time it is requested, so it needs to keep, for instance, a website on to access your game, a hacker doing a DDoS on your website (website is down) shouldn't happen based on the Availability principle. 

But the Availability does not mean that the information need to be at all times available, just when it need and to those who should.

### So How Were Protections in the Past?

Around the 1960s, there were only passwords and Physical protections, That means that the computer (accessible mostly on Universities) have a room with a lock on it that protects the computers to be accessed. 

This makes things very physical and did not have something like antivirus or even the idea of Virus. But did not take too long to that be a reality.

Around the 1970s, there was the concept of Antivirus and Virtual protection, especially because the ARPANET was taking force around the country of the USA. But specially because a man called Bob Thomas created the first known virus: The Creeper.

![Image description](/images/unity3d-csharp-and-string-security/2.jpg)

Bob Thomas created a program that auto-replicated itself and spread through the network of computers, it did nothing harmful, just printed the text "I'M THE CREEPER: CATCH ME IF YOU CAN" on the screen.

The consequence of this creation, makes Ray Tomlinson to create a new program that eliminate the creeper program called "Reaper".

With that, we had our first virus (Creeper) and antivirus (Reaper).

Ok, The Creeper was actually a Worm, a self-propagate program through the network, but these names (virus, antivirus, worms) was not a thing until 1980s where the personal computers and as we know the internet was around.

With time, virus, worms, antivirus and similar has become more and more complex. Nowadays, information is more valuable than anything, and we need to secure it the proper way.

### HACKERS!?

![Image description](/images/unity3d-csharp-and-string-security/3.jpg)

Hackers from the definition written by Wesley Chai:

"A hacker is an individual who uses computer, networking or other skills to overcome a technical problem. The term also refers to anyone who uses their abilities to gain unauthorized access to systems or networks in order to commit crimes. A hacker may, for example, steal information to hurt people via identity theft or bring down a system and, often, hold hostage in order to collect a ransom.

The term hacker has historically been a divisive one, sometimes being used as a term of admiration for individuals who exhibit a high degree of skill and creativity in their approach to technical problems. However, the term is also commonly applied to individuals who use this skill for illegal or unethical purposes."

So, many words, but we need to talk about some of the words used in this:

Hacker, before the cybersecurity term applied, was used for people who tinker code or make some mess that work for a program, or a clever solution for a limited system.

Hacker commit crimes is a misleading phrasing, we should separate in hats:

**White Hat** : Ethical Hacker, do not commit crime
**Black Hat** : Uses hacking for unauthorized access (usually cybercrime)
**Gray Hat** : Uses hacking for his own benefit, maybe some TI worker at day and cybercriminal at night for his own good

Wrapping it up, hackers can be beneficial finding security flaws on your system, your company (with permission to test your system, obviously) and find ways to make it more secure. But the same way can be beneficial, it can be harmful with cybercrime, stealing information, spying on unaware users, etc.

---

## What is Compilation and Decompilation

When you, yes You, the game developer, makes a code for a game and need to release to the public (making an .exe for example) you need to make a compilation of your code. For simplicity, we will define as:

**Compilation**:
C# Code → To → Machine Code

The machine code is not readable by developers/humans. But if we have only the compiled code, and we need to read it, the process needed is called **Decompilation**:

**Decompilation**:
Machine Code → To → C# Code (Human Readable)

For this case, we will only make readable the C# code from the original program. Usually, the Decompilation must have the possibility to recompile the code to an executable again. That's the real goal.

---

## A simple decompilation and password reading

![Image description](/images/unity3d-csharp-and-string-security/4.jpg)

When you pass through the decompilation process, maybe the final code is not the same as the original, but it surely acts like the original one. That's because in the compilation process the code (C#) need to be converted in another language to be processed to machine language, some (mostly) C# features are not present and need to be adapted.

In the example above, we have a original code that check if the password in a variable equals "S3Cr37"

```csharp
void Start()
{
   if(pass == "S3Cr37")
   {
      Debug.Log("you have access");
   }
}
```

and after we passed through the process of decompilation, the code is something like:

```csharp
void Start()
{
   if(!(this.pass == "S3Cr37"))
      return;

   Debug.Log((object) "you have access");
}
```

Kinda different, but they do the same thing, the low level code works as "Jumping from one line to another" so the decompiled code prefers to exit the method on a failed verification (that explains the `return;` call);

But we must not miss the point, our "Secret Password" is still visible, this could be your secret password, secret key or Secret Address for a page.

So, before understanding how to prevent, let's see how it works.

---

## What is Dotpeek and How to use with Unity

We are using Unity as an example for this exercise, if you need, I'll be uploading the [example project and the source code](https://github.com/Falme/Unity3D-String-Hack) in the GitHub (But it's so simple you could do it yourself).

We will create a Unity project with the standard configurations, add a script to the default scene called "Secret.cs" with our secret code. Just that and make a Windows build (sorry Linux users D:)

[Here's our build](https://github.com/Falme/Unity3D-String-Hack/releases/tag/Build), Let's crack this Secret.cs;

**But Hey, this is just amateur insecure stuff, right?**
- Not at all, many big games that you know have this simple insecure thing that make you able to read their code, here are some examples (Do Not do anything with these open codes):

![Image description](/images/unity3d-csharp-and-string-security/5.jpg)

(Take note about INSIDE, that have a +/-, this is a special case, we will talk about it as well)

The real danger is about the secret content that should not be accessible to the public/players.

With the build, now we need to download a tool called [DotPeek](https://www.jetbrains.com/pt-br/decompiler/) from JetBrains. The tool is a decompiler, so we can disassemble the .DLLs from the Unity build.

### Quick Stop to explain how DLLs works with Unity

![Image description](/images/unity3d-csharp-and-string-security/6.jpg)

For default, all the scripts that you (developer) create in the engine will be compiled in an Assembly Definition called **Assembly-CSharp.dll**, remember this name, it will be important in the future.

This makes Engine Unity Code stable and separated from the developer code. If you don't want all your code in one DLL, you'll need to create an Assembly Definition in your project to make it separated, but the references STILL points to Assembly-CSharp.

Ok, back to hacking.

### After finished downloading DotPeek

![Image description](/images/unity3d-csharp-and-string-security/7.png)

After you download Dotpeek, you will need to find out if the target build is compatible to this attack. To know that, you'll need to find the build folder and find a DLL in the path:

```
\PathToBuild\BuildName\BuildName_Data\Managed\Assembly-CSharp.dll
```

If this path and DLL exists, your attack can be done. This happens because Unity have a standard option in Player Settings > Other Settings > Configuration > Scripting Backend called Mono

![Image description](/images/unity3d-csharp-and-string-security/8.png)

It makes build much faster, but sacrifices part of other things like performance and security. 

So, enough talking, open your DotPeek, and select "File > Open" and select your target **Assembly-CSharp.dll** (I told you this was important):

![Image description](/images/unity3d-csharp-and-string-security/9.png)

![Image description](/images/unity3d-csharp-and-string-security/10.png)

![Image description](/images/unity3d-csharp-and-string-security/11.png)

After loading the project, you'll select the Assembly-CSharp field, right-click and select the Export to Project..., select a folder and wait for the decompiler to finish working.

![Image description](/images/unity3d-csharp-and-string-security/12.png)

After exporting, you will have the C# files from the original project to be human-readable (kinda different, but readable).

![Image description](/images/unity3d-csharp-and-string-security/13.jpg)

Congratulations, you got the secret password!

But wait, how can I prevent this?

---

## Unity with IL2CPP and PlayMaker

A way to make things a little difficult for the person who is opening your code is to change the **Script Backend** to **IL2CPP (Intermediate Language To C++)** At "Edit > Project Settings > Other Settings > Configuration"

![Image description](/images/unity3d-csharp-and-string-security/14.png)

What it does is not put your code directly in to a DLL, but converting your C# code a Intermediate Language (Microsoft Intermediate Language) to C++ and then compiling to machine code.

Making this process, it will not create an Assembly-CSharp.dll

![Image description](/images/unity3d-csharp-and-string-security/11.png)

### No problem with that?

Yes, there's a problem, and it's that a String is still a String. We will find our secret code again, but now with some Linux Bash commands.

With our new files compiled to the build, we need to go to the file "global-metadata.dat" at 

```
\PathToBuild\BuildName\BuildName_Data\il2cpp_data\Metadata\global-metadata.dat
```

We will use [WSL (Windows Subsystem for Linux)](https://apps.microsoft.com/store/detail/windows-subsystem-for-linux/9P9TQF7MRM4R), that's a Linux inside your Windows 10/11, if you don't have it, you can download it in the Microsoft Store, it's free.

With WSL, we will use this command to our file global-metadata:
```bash
strings global-metadata.dat | grep S3Cr37
```

Where:
- `strings` lists all strings inside a file
- `grep` Highlight/Only Show if an argument is true/found
- `S3Cr37` is the argument we want to find

With that, we can see that our Secret Code can be found in our build.

![Image description](/images/unity3d-csharp-and-string-security/15.jpg)

Well, of course we knew beforehand what was the code we need to find. But If we knew from previous builds what words to discard or if we use a wordlist to find relevant values, we can still find some interesting results, including our code.

### Time for PlayMaker

First, I'm not saying that PlayMaker is safer, this is just a curiosity that make sense in this document.

[PlayMaker](https://assetstore.unity.com/packages/tools/visual-scripting/playmaker-368) is a tool for Visual Scripting, similar to Blueprints for Unreal, but not official for Unity. The curious case is that INSIDE uses it, and this makes their code not convertible to code directly, but converted to Assets.

![Image description](/images/unity3d-csharp-and-string-security/16.jpg)

This makes it harder to find information in the code from the previous techniques that we used.

The conversion of your code to Asset can also be used in different ways like:

1. Using Scriptable Objects: Where you put your key to an Asset
2. Using PlayMaker or Other Visual Scripting : Same as above, but this influences your developer team
3. Using External Services : Still using Scriptable Objects, but this one calls a service outside, keeping your data/access outside the game (We will talk about this one later)

In the case of the PlayMaker, what it does is to convert the visual scripting data into a **resources.assets** data. So if you decide to open the resources.assets file in a `strings` bash command (as we talked above) you will only get rubbish.

![Image description](/images/unity3d-csharp-and-string-security/17.jpg)

### So, can it be opened?

Yes... Kinda... you'll need some fancy study and tools to do that, what I can tell you is how to show Assets in a build.

We will use a tool called [AssetStudio](https://github.com/Perfare/AssetStudio). It's free and can be accessed in a Github repository.

As this example, we will take a project with PlayMaker and open it with AssetStudio:

![Image description](/images/unity3d-csharp-and-string-security/18.jpg)

As shown in the above image, we can find, open and export these PlayMakerAssemblies and PlayMakerFSM files to our system and maybe find something useful. I'll stop here about the PlayMaker, but if you want to know more, play with it, make your PlayMaker example code and try to find some secret content.

Bonus: AssetStudio is also used to find images, sounds, music, models, textures and every other thing that Unity categorize it as an Asset, that can be exported and read. That's how many models and information can be found so fast after a launch of a game.

---

## Inevitability and Git

So... What we learned until now?

- All options above was terrible
- No code is 100% secure
- What we are doing is only delaying the hacker to find the key

This can be resumed with the "Infinite Monkey Theorem" (Hiding the Monkey identity for security):

![Image description](/images/unity3d-csharp-and-string-security/19.jpg)

The theorem says that _a monkey hitting keys at random on a typewriter keyboard for an infinite amount of time will almost surely type any given text, including the complete works of William Shakespeare_.

With our case, what the hackers have is time, so, with time and patience, the hacker will find your hiding key (also it's not only one monkey if your game is published in Steam).

### So how the hackers can find my key? 

Here are some examples:

1. **GitHub and Bitbucket files**: They can access/hack the .git files at some point and going right in the source of your game, and this is not impossible, many games AAA had this incident, the Git example is only because there's also history of all your commits, making possible other attacks (maybe?)

2. **Two-Factor Authentication and Obsolete login methods**: Some users don't have Two-Factor Authentication, on GitHub, on Unity, on Google Play Store, on Steam, you name it. This makes sure that the login need your authentication. [Here's a funny video of Gabe Newell asking to be hacked, but the Two-Factor Auth blocks it.](https://www.youtube.com/watch?v=gYs9nS8LlZ8)

3. **Low-Hanging Fruit**: Making the basics is essential, but hackers love to go to those who don't do the basics, because it's easier to hack. So if you don't do the simple basics (like change to IL2CPP) they will use an automation code to find you faster.

4. **Encrypted Strings and Code**: Making encryption is good for everyone, to send email or to keep an access secret. But make sure to not keep the key de decrypt in the same drawer as the locker and make sure your encryption is good, because if it's just a ROT13 encryption, it will not make a difference.

### Any other ways to make it more secure?

1. **Use External Services like [PlayFab](https://azure.microsoft.com/pt-br/products/playfab)**: PlayFab is a service where you can put data, rankings and actually remote code, so if you need to send a score to a competitive game, you can have remote code to check if this is a correct (or viable) score to be added, but more than that, check if the transaction is legit, looking through data of the player and, if needed, banning them. This way your key/service is not in your game, so your build is safe.

This remote content can also be applied to game events content, where you can put a Asset Bundle (or Addressables) in a CDN server/service and release when the time has come, so the content Assets is not in your build (yet)

2. **Permissions for Reading and Modifying**: Make sure that some data in the server (sending requests to a server) cannot be modified if it should not be modified. It's easy to not pay attention to that because we don't think of the day when someone will delete/modify our data, but make sure this is true.

3. **Do the basics and more**: I see other fields like Software development or Web Development being so cautious about security (ok, not that much, but they make the basics) and game development make no case about that. And sometimes the solution is so simple. So take care of your project, do not keep any secret that shouldn't be read in your builds, specially if it is online, triple check it.

---

## Conclusion and Recommendations

Let's recap:

1. Unity Builds with Script Backend as IL2CPP
2. No compromising strings inside Scripts
3. Use recommended protection for Git Servers (GitHub, Bitbucket, etc.) with [SSH RSA Keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
4. If you need to register a value, use external services that have an authentication layer

### Other Security Recommendations outside GameDev

1. Do not open suspicious Links or Emails (even from friends)
2. Activate [Two-Factor Authentication](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication) for every service (Steam, Github, Google, etc...)
3. Use Antivirus (better than nothing)
4. Check if your password had been exposed (https://haveibeenpwned.com/)
5. Do not use personal accounts inside your company
6. Keep all softwares programs Up to date (specially Windows)
7. Common sense

### Bonus: Can it be useful in Game Development?

Yes! This is a thing very common nowadays (opening builds and checking for secrets), you, as a game developer, can use this to add secret content in your games, or making a game where you NEED to open the game to find clues. But mostly is a content that is not necessary for the gameplay, just a joke for the hardcore fans.

And you can do it as well.

---

## References (And other links)

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
