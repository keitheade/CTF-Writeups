# CTF-Writeups!
----
## OSINT
----
### Honk Honk
----
[image](https://user-images.githubusercontent.com/36986239/192222093-ac5350bd-6950-47eb-abb8-ce8c7a040fa3.png)

Time to check the rego sites...
Vic - N0
NSW - YES

![image](https://user-images.githubusercontent.com/36986239/192222395-8322fc1b-0453-40a9-8017-da3695f06849.png)

`DUCTF{19/07/2023}`

----
### Does it Fit My CTF?
----
![image](https://user-images.githubusercontent.com/36986239/192222728-ad498e2a-0a6f-4b60-871e-5ff9b7bac187.png)

lets look up the car with youtube…

![image](https://user-images.githubusercontent.com/36986239/192222794-8bac7c7f-e20d-46d7-8a97-bb175cbaeb2b.png)

Time to check out their "About" info...

![image](https://user-images.githubusercontent.com/36986239/192222868-6f8c4ead-8035-4417-b354-428a51bc5349.png)

`DUCTF{MightyCarMods}`

----
## DFIR
----
### doxme
----
![image](https://user-images.githubusercontent.com/36986239/192222968-84bed782-bd8c-420f-a358-722a9508d328.png)

Download file
what is it??

![image](https://user-images.githubusercontent.com/36986239/192223005-a39b31d9-91f6-4e2f-ad42-600272a775ac.png)

so it’s office file… open in zip and look at some pictures :D
image1.png and image2.png (in the /word/media/ folder)

![image](https://user-images.githubusercontent.com/36986239/192223062-26b19711-bbac-47ef-aa99-492ba40fc2ec.png)

`DUCTF{WOrd_D0Cs_Ar3_R34L1Y_W3ird}`

----
### Shop-Setup&Disclaimer
----
![image](https://user-images.githubusercontent.com/36986239/192223182-cbf7d309-ce2f-4a78-b6e6-a2022a15ed9a.png)

`DUCTF{IAgreeToTheTeasAndTheSeas}`

----
### Shop=Knock Knock Knock
----
![image](https://user-images.githubusercontent.com/36986239/192223357-f1c59642-9419-4ec5-a497-1bdc11c8892f.png)

Load the file into splunk
search the index=ducft useragent="curl/7.64.1"
identify the origin ip address

![image](https://user-images.githubusercontent.com/36986239/192223401-0a413660-1f82-4871-8df5-5851b2020e5b.png)

Source ip address `58.164.62.91`

Look that up with [www.whois.com]{:www.whois.com}

![image](https://user-images.githubusercontent.com/36986239/192224067-60bcd5a4-348a-4c27-8872-0d2ca8055fe5.png)

`DUCTF{abuse@telstra.net}`

----
### ogres are like onions
----
![image](https://user-images.githubusercontent.com/36986239/192225459-be659715-d984-4830-8574-8f378f50d40f.png)

Ok so I have no idea on docker forensics…..
soo
google
ok … lets look at some of the history as the flag is conveniently missing

![image](https://user-images.githubusercontent.com/36986239/192225534-becd5062-1495-419f-9d25-b073d5d94651.png)

So why is it missing…

![image](https://user-images.githubusercontent.com/36986239/192225584-6bf80435-26f5-428d-81c8-8e8372f1c410.png)

cause they deleted it… nice…
ok more googling
There is a thing called Docker-exporer (time to download that https://github.com/google/docker-explorer)
Lets see all these layers….

![image](https://user-images.githubusercontent.com/36986239/192225839-bf29ae8a-2b2c-48cc-a5f3-9e1788b97e5f.png)

time to manually look as im not skilled enough to know where to look lol

![image](https://user-images.githubusercontent.com/36986239/192225888-c15cff10-9b6f-42b9-a767-82e297216ff3.png)

to save you the time of reading.. .this is the one

![image](https://user-images.githubusercontent.com/36986239/192225917-822c4073-fe8d-4cfa-ae53-de3e8eccc1b1.png)

Lets open that bad boi up..

![image](https://user-images.githubusercontent.com/36986239/192225948-e0ba7e08-4fad-495a-a3f7-61d817a888f8.png)

Sigh… thanks for that….

`DUCTF{P33L_B4CK_TH3_L4Y3RS}`

----
### Investigation 1
----
![image](https://user-images.githubusercontent.com/36986239/192246025-fe2d1833-b7d0-40f2-8975-278abc8db827.png)

Download file and open the AD1 up in FTK imager

![image](https://user-images.githubusercontent.com/36986239/192246064-f8c4d3d3-0384-4b24-944d-e8040108d7d1.png)

ok so this took a lot of time…. was looking throuh all the registry keys for scheduled tasks,
powershell commands to see…
but nothing… then I remembered that persistance can be achieved by wmi…
so time to google…
the file I needed to open was in `\C___NONAME [NTFS]\[root]\Windows\System32\wbem
\Repository OBJECTS.DATA`
How to open it?

<https://github.com/davidpany/WMI_Forensics>

a lovely genious from mandiant

so I downloaded the PyWMIPersistenceFinder.py
***Note this needed to be run by python2***

![image](https://user-images.githubusercontent.com/36986239/192246518-4783c76f-4bf7-4da1-b36d-cffb39072d09.png)

First we can see this is a scheduled task…
The time we found
Part 1: `12:38`

so lets decode the powershell script used `From Base64` to decode > `Decode text` UTF-16LE
because its powershell > `Generic Code Beautify` to lay it out nicely > `Syntax hightlighter` to
highlight the code for easy use

![image](https://user-images.githubusercontent.com/36986239/192246700-f6cc1140-9ba2-4e5b-9829-97474d51eb50.png)

there is an interesting variable that is encoded…. `$ser`

![image](https://user-images.githubusercontent.com/36986239/192246763-7a5742a7-fa08-41d9-906c-740406d10056.png)

so lets decode that further… Again, `From Base64` to decode > `Decode text` UTF-16LE
because its powershell

![image](https://user-images.githubusercontent.com/36986239/192246896-0625a9db-9cc0-48b0-ab3a-0d20b1a639a6.png)

Now we have the URL / IP address
Part 2: `192.168.0.27`

`DUCTF{12:38_192.168.0.27}`

----
### Investigation 2
----
![image](https://user-images.githubusercontent.com/36986239/192247395-1d049ec7-467a-421b-974b-d30cf26e4952.png)

We lets open the NTUSER.DAT in Registry Viewer.
**NTUSER.DAT**: `HKCU\Software\Microsoft\Windows\Shell\Bags\1\Desktop`

![image](https://user-images.githubusercontent.com/36986239/192247545-6b367c0c-3323-4d98-bd32-9f21e09b743b.png)

![image](https://user-images.githubusercontent.com/36986239/192247563-55b31417-f417-46e8-ac1f-fc73ad6f26bb.png)

So I know the file exists… now when was it opened by the user…
This took me SOOOOooo many guesses…. then rethought it… time to look for recent files…
So to create the PECmd output I went to good ol Eric Zimmerman (the prefetch parser)
`.\PECmd.exe -d 'Z:\C___NONAME [NTFS]\[root]\Windows\Prefetch' --csv .\csv`

![image](https://user-images.githubusercontent.com/36986239/192247684-562208ad-b126-4b59-9c4d-9ee32e8698df.png)

Then import it into timeline explorer (again another Eric Zimmerman tool)
then filtered on **passwd**
Found the lnk file that showed when it was last accessed by the user **Challenger**

![image](https://user-images.githubusercontent.com/36986239/192247772-c211a3bc-7f70-4dae-881c-b41ba8847554.png)

`DUCTF{08:25:27_R3sident!al}`
