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

Sometimes I find very interesting the puzzle of Capture the Flag (CTF), where I can use some of my time to dive into a problem that I have no idea of "How the heck am I going to solve this?". And then, after some hours, in bed, I get up saying "I know!!" and try my new idea, maybe it's right, maybe it's wrong, but it's very satisfying.

This one is about the CTFChallenge Website, more specific the Vuln Begin Challenge, with 9 Flags to be found. 

In my case, the website will be relative from time to time (every time a new instance of the CTFchallenge), so, for this document I'll refer to the URL as "mymachine.vulnbegin.co.uk/".

So, Here's my journey to find all the flags:

---

## Flag 1

![Image description](/images/ctfchallenge-vulnbegin-flags/1.jpg)

As the website says, "There's not much here!", no effective button, no form, no flags inside the Inspect element, So I need to do alternative ways to find the flags.

Usually I start looking for robots.txt, which maybe have some files or folders paths, usually used to filter these folders to not allow to be found from robots (like the Google search) and keep it secret.

So I looked in mymachine.vulnbegin.co.uk/robots.txt to see if it has some hidden file/path

```
User-agent: *
Disallow: /secret_d1rect0y/
```

So, with that info, there's a disallowed folder called /secret_d1rect0y/ . I go to the address and I have found the first flag:

{{% details title="Answer:Flag 1" open=false %}}
```
[^FLAG^2B22E2CB70E218510802B0359488F6A2^FLAG^]
```
{{% /details %}}

---

## Flag 2

This flag was kinda problematic, I get the idea, but I will address the problem soon.

This one need to find a subdomain in the mymachine.vulnbegin.co.uk, but the subdomain is not easy to be accessed from bruteforce or wordlists. 
For that we need to go to the `crt.sh` to search the Certificates from the `vulnbegin.co.uk`;

![Image description](/images/ctfchallenge-vulnbegin-flags/2.jpg)

This image show a problem, that the address in the input is `http://vulnbegin.co.uk/`, but with the `http://` or `/` at the end will not find the certificates that I need, and I wasted a lot of time in this simple string problem.

after changing it to only `vulnbegin.co.uk` I got the correct results with the subdomain that I was looking for:

![Image description](/images/ctfchallenge-vulnbegin-flags/3.jpg)

With that I found the `v64hss83` subdomain, and accessing the page `v64hss83.mymachine.vulnbegin.co.uk` we access the Flag:

{{% details title="Answer:Flag 2" open=false %}}
```
[^FLAG^047524FE61AE6B5FD1D184994C7322FC^FLAG^]
```
{{% /details %}}

---

## Flag 3

Finding the subdomains of mymachine.vulnbegin.co.uk/, can be useful, and it's recommended in the useful tools page the FFuF to enumerate the subdomains, for that, I used the command

```bash
ffuf \
-w subdomains.txt \
-u http://FUZZ.mymachine.vulnbegin.co.uk
```

Where:
- `ffuf` calls the application 
- `-w subdomains.txt` use a wordlist to scan 
- `-u http://FUZZ.mymachine.vulnbegin.co.uk` define the address to enumerate, where FUZZ word is the placeholder to test possible correct values 

With that, it returns with a possible subdomain called http://server.mymachine.vulnbegin.co.uk

![Image description](/images/ctfchallenge-vulnbegin-flags/4.jpg)

Checking the page, we find the flag:

{{% details title="Answer:Flag 3" open=false %}}
```
[^FLAG^E858ED9649E57BECE9ACD1A4C60D3446^FLAG^]
```
{{% /details %}}

---

## Flag 4

The CTFChallenge Useful tools recommends the use of DNSRecon to check information about the mymachine.vulnbegin.co.uk/ DNS. So I run the 
```bash
dnsrecon \
-d mymachine.vulnbegin.co.uk \
-D ./subdomains.txt \
-t std --xml dnsrecon.xml \
--lifetime 5.0
```

Where:
- `dnsrecon` calls the application
- `-d mymachine.vulnbegin.co.uk` set the domain to check
- `-D ./subdomains.txt` uses the wordlist subdomains from the Useful tools page and bruteforce for information
- `-t std` call for standard scan
- `--xml dnsrecon.xml` export results to an XML called dnsrecon.xml
- `--lifetime 5.0` keeps the lifetime of scan to 5.0 seconds

With the scan, it reveals new information about the DNS and with that, the Flag

![Image description](/images/ctfchallenge-vulnbegin-flags/5.png)

{{% details title="Answer:Flag 4" open=false %}}
```
[^FLAG^BED649C4DB2DF265BD29419C13D82117^FLAG^]
```
{{% /details %}}

---

## Flag 5

After Calling the FFuF to find some hidden pages inside the mymachine.vulnbegin.co.uk/* I have found a page called cpadmin:

![Image description](/images/ctfchallenge-vulnbegin-flags/6.jpg)

Checking this page, we can find a login page:

![Image description](/images/ctfchallenge-vulnbegin-flags/7.png)

So the first thing to do is to do the basic login test: Put admin/admin in login and password. My surprise is that the result tells me that is an invalid password, but not login, so there's an admin user, I just need to find the correct password:

![Image description](/images/ctfchallenge-vulnbegin-flags/8.png)

For that I'll use Hydra to brute force the password, the command 
```bash
hydra \
-l admin \
-P ~/wordlists/passwords.txt \
mymachine.vulnbegin.co.uk \
http-form-post "/cpadmin/login:username=^USER^&password=^PASS^:Password is invalid" -V
```
will do the work.

Where:
- `hydra` calls the program for brute force
- `-l admin` set the login fixed as the word 'admin'
- `-P ~/wordlists/passwords.txt` calls a password wordlist to test
- `http-form-post` is the method-form for hydra to attack
- `/cpadmin/login:` is the path to the login page address
- `username=^USER^&password=^PASS^` is the information body to send to login form requisition, where ^USER^ will be replaced by the user value and ^PASS^ will be replaced by the password value
- `Password is invalid` is the identifier in page to check if the login was successful or not, in this case we are only checking for the password validation, as we know the login value
- `-V` verbose flag, so we can see all the hydra attempts

After some tries, we found our secret password for login:

![Image description](/images/ctfchallenge-vulnbegin-flags/9.jpg)

after inputting the correct password, we will be presented with the Flag:

![Image description](/images/ctfchallenge-vulnbegin-flags/10.jpg)

{{% details title="Answer:Flag 5" open=false %}}
```
[^FLAG^93D7491FB4B054FB5C5AC3E0292BE41C^FLAG^]
```
{{% /details %}}

---

## Flag 6

After making a login as Admin in the cplogin page, it says that a configuration file was available, so I will use a wordlist to find new content with the session cookie from login. 
To do that I'll use FFuF to Fuzz the content of the page with a Cookie Header:

```bash
ffuf \
-w ~/wordlists/content.txt \
-u http://mymachine.vulnbegin.co.uk/cpadmin/FUZZ \
-H "Cookie: token=2eff535bd75e77b6c70ba1e4dcb2873"
```

Where:
- `ffuf` calls the program
- `-w ~/wordlists/content.txt` uses a wordlist called content
- `http://mymachine.vulnbegin.co.uk/cpadmin/FUZZ` is where I want to find new content and FUZZ is the string to be tested
- `-H "Cookie: token=2eff535bd75e77b6c70ba1e4dcb2873"` is the Cookie session to make the page think that I am logged in (actually the session cookie IS the thing that checks if I am logged in)

After making through all the wordlist, we found some pages:

![Image description](/images/ctfchallenge-vulnbegin-flags/11.jpg)

Let's check the `env` page:

![Image description](/images/ctfchallenge-vulnbegin-flags/12.png)

With that we found the Flag and some hint for the next flag

{{% details title="Answer:Flag 6" open=false %}}
```
[^FLAG^F6A691584431F9F2C29A3A2DE85A2210^FLAG^]
```
{{% /details %}}

---

## Flag 7

The address `server.mymachine.vulnbegin.co.uk` has returned "Not Authenticated" from some previous flags, maybe with this new Header parameter `"X-Token: 492E64385D3779BC5F040E2B19D67742"` we can be authenticated in the server subdomain. 
So using the command 
```bash
curl http://server.mymachine.vulnbegin.co.uk/ \
-H "X-Token: 492E64385D3779BC5F040E2B19D67742"
```
and the new X-Token in the Header we can be Authenticated, and with that a new flag appears:

![Image description](/images/ctfchallenge-vulnbegin-flags/13.jpg)

{{% details title="Answer:Flag 7" open=false %}}
```
[^FLAG^0BDC60CC5E283476E7107C814C18DCCF^FLAG^]
```
{{% /details %}}

---

## Flag 8

With the X-Token auth value, we can re-scan for new addresses, but now as the Host being the server. 
So calling the FFuF again to find content in the page but with 2 values on header, one being the X-Token to authenticate the access `-H "X-Token: 492E64385D3779BC5F040E2B19D67742"` and the Host as server `-H "Host: server.mymachine.vulnbegin.co.uk"`. 
```bash
ffuf http://mymachine.vulnbegin.co.uk/user \
-H "X-Token: 492E64385D3779BC5F040E2B19D67742" \
-H "Host: server.mymachine.vulnbegin.co.uk"
```
With these header values, we find a new access at /user/:

![Image description](/images/ctfchallenge-vulnbegin-flags/14.jpg)

Accessing the /user/ page we find a JSON with the following information:

`{"id":27,"endpoint":"/user/27"}`

And accessing the /user/27/ page we find another JSON with the following information:

`{"id":27,"username":"vulnbegin_website","endpoint":"/user/27/info"}`

And accessing the /user/27/info page we find another JSON with the Flag information.

{{% details title="Answer:Flag 8" open=false %}}
```
[^FLAG^7B3A24F3368E71842ED7053CF1E51BB0^FLAG^]
```
{{% /details %}}

---

## Flag 9

In the last Flag there was ID information in /user/27/info, that means that a user have the identifier in the Database as 27. That reveals that other users may be available for reading information.
The idea is to loop through the users until we find something useful or ideally the Flag.

For that I'll use a loop from bash to gather information with curl:

```bash
for i in {0..5}; \
do curl http://mymachine.vulnbegin.co.uk/user/$i/info \
-H "X-Token: 492E64385D3779BC5F040E2B19D67742" \
-H "Host: server.mymachine.vulnbegin.co.uk"; \
done
```

Where:
- `for i in {0..5};` make it loop the next action 6 times (starts at zero, finishes at 5)
- `do` defines what action to do (next command will loop)
- `curl http://mymachine.vulnbegin.co.uk/user/$i/info -H "X-Token: 492E64385D3779BC5F040E2B19D67742" -H "Host: server.mymachine.vulnbegin.co.uk";` The command we saw last time, but now with $i in the place of the user ID, this will change to the loop number (from 0 to 5)
- `done` identifier of finishing the loop command

After looping 6 times, we have our flag at ID 5:

![Image description](/images/ctfchallenge-vulnbegin-flags/15.jpg)

{{% details title="Answer:Flag 9" open=false %}}
```
[^FLAG^3D82BE780F46EE86CE060D23E6E80639^FLAG^]
```
{{% /details %}}

---

## Conclusion

This was a journey, many of the solutions I've seen before, but the CTF that have a correct sequence to find a flag, and other flag, and another flag... It's kinda motivating to find the next one (I also found 2 flags on VulnLayers, the next free CTF, but I'll not finish it so soon). 

The CTF Challenge gives me a feeling of Escape the room games, where I find the solution of a puzzle, but also gives me a hint to what should I look for next. 

But in the negative side, if I find the answer of a puzzle in a wrong order, it can be very confusing to finish the CTF/Escape the room as a whole. In the CTFChallenge case, I skipped the Flag 2, and found the result of Flag 3, 4 and 5, this makes the line of thought very hard to follow.

As a lesson, I need to have more patience and make my steps very clear, one foot after another till I reach the end.
