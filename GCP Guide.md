
# How to host your modmail bot on Google Cloud Platform for free | Made by [@mesub7](https://github.com/mesub7)

Want to host your modmail bot for free? Don’t want to be ridiculed for using Heroku, Glitch or repl.it? This guide will take you through how to get the bot running on Google Cloud.
**DISCLAIMER: I am not responsible for any charges that you face due to you digressing from the guide.**

**(Need to update your installation? Check [this.)](https://github.com/Dragory/modmailbot/blob/master/docs/updating.md)**

**Highly recommended: Configure from the host to save time and to minimise errors**
If you know your way a little bit around Linux and want to just download and configure the bot remotely, you can use the Terminal instructions found [here](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#terminal). It's quicker but can be confusing if you're new to server administration as there is no graphical interface (buttons to press).

Due to the multiple services/applications that we are using, this guide may be outdated. If this is the case then please let me know here or in the discord server.
I am aware that there are a few alternative methods to this. If you know what you are doing, then by all means use part of this guide and your own method. Just note that if you do this, I can’t really provide support as your method may be the reason it is not working.

This guide presumes that you have basic terminal/command prompt knowledge and you are running Windows 10. If you don’t know your `dir` from your `cd` then it might be worth getting somebody else (not me) set up this for you! Google has all the information that you need. I cannot be sure if the steps are different in other operating systems, use your own judgement for equivalents. 

So, with that out of the way, let us begin!

## Step one: Discord Developer Portal
1)	Navigate to https://discord.com/developers/applications and click new application.
2)	Give it a name.
3)	Go to the bot section (which is found on the left hand navigation pane) and click add bot.
4)	Confirm your choice.
5) Ensure that the **Server Members intent** is enabled.
![image](https://mesub.is-ne.at/STWMKt.png)
6) (Optional) Customise your bot. You can change the name and add a profile picture if you wish. You can also make the bot private (so only you can invite it). **Do not change the OAUTH2 code grant setting.**

Don’t forget to save!

We’ll be coming back to this later so make sure that you keep this tab open!

## Step two: downloading and configuring the bot
1)	Navigate to https://github.com/Dragory/modmailbot/releases and download the latest version (as of me updating this it was v3.0.3).
2)	Download the zip and extract it to a folder.
3)	Copy and paste the `example.config.ini` file and rename it to `config.ini`.
![image](https://user-images.githubusercontent.com/49169805/87231360-7dc4d980-c3ae-11ea-86c4-8a59e6199bb0.png)
4)	Open the config.ini file and edit it to your liking. You’ll need the bot token from the developer portal as well as a `mainServerId` and a `inboxServerId`.
For information on what can go in the config file, check [the official documentation.](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md)

![image](https://user-images.githubusercontent.com/49169805/87231371-9a611180-c3ae-11ea-8a1b-b560e9a4f08a.png)

5)	Save the config file.
6)	Make sure you know where you’ve kept this folder, you will need it later on.

## Step three: Google Cloud Platform, node.js and pm2
This is where we’ll be hosting the bot:
1)	Navigate to https://cloud.google.com/free and select “Get started”.
2)	Enter your card details.
_But I thought it was free?_
It is free, but for verification purposes, Google needs it. You will not be charged unless you upgrade to a billing account.

_But I [insert excuse here] and don’t have a card!_
I figured as such. Below, there is a flowchart that goes through what processes I think is feasible.
**NB: To save me time, credit/debit card == bank account (If you’ve got a bank account then it should accept it).
Additionally, if you live in the US then Google also accepts PayPal.
Consult: https://cloud.google.com/billing/docs/how-to/payment-methods for more information.**

![image](https://user-images.githubusercontent.com/49169805/87231445-34c15500-c3af-11ea-864a-3f86b7319781.png)

 

3)	Go to compute engine.
4)	Click create (when the option becomes available). ![image](https://mesub.is-ne.at/ABsVzK.png)
5)	**Follow this very carefully if you want it to be free:**

a.	Give it a name.
Select one of the following zones: **us-central1 (lowa), us-east1 (south carolina) or us-west1 (oregon).** I’m picking us-east1 as it is closest to Discord's API servers. I would leave the zone but change it if you wish.

  b.	Machine configuration should be:
  **General Purpose Series: N1.**
  
  **Machine type: f1-micro (1vCPU, 614MB of memory).**
  
  **Bootdisk: Debian, CentOS or Ubuntu.**
  
  **Leave the disk type to: standard persistent disk.**
  
  **Disk size: 10-30GB (I’m using 25GB).**
  
  **Leave the Identity section.**
  
  **Firewall: Allow both HTTPS and HTTP traffic.**
 
 If you did it all correctly then it should tell you that your first 744 hours (or something along those lines) are free (This would be on the side of the screen).

![image](https://mesub.is-ne.at/ZXQAQB.png)

6)	Click create! Once it has loaded it’ll look something like this: ![image](https://mesub.is-ne.at/o3ld33.png)

7)	Click the SSH button to view your instance. It should be a command line. ![image](https://mesub.is-ne.at/SECrhd.png)

8)	Run `whoami` and note the output. It is **very important** (to avoid user account issues). ![image](https://mesub.is-ne.at/rDJ9sU.png)

### nvm, node.js and pm2
We need the node version manager and node.js for the bot and we need pm2 to keep it on all the time, so we’ll take the opportunity to get them now. 
1)	Navigate to https://github.com/nvm-sh/nvm#installing-and-updating

2)	Copy the first `curl -o-…` command (from the page). This will run the install script (doing all the dirty work for us). :P

3)	Close and reopen your terminal (the window).

4)	Run `nvm` to verify that it has been installed.

Now to install node.

5)	List the versions of node available by running `nvm ls-remote`.

Now (at the time of writing) since the bot supports versions 12 to 14, for compatibility's sake, we'll install the latest version of V12 (feel free to install any version between 12 and 14).

6)	Run `nvm install 12.19.0` (or whatever version you want that's between V12 and V14 (like 14.13.1 for example).

7)	Optional: run `nvm ls` to verify that it is installed.

Now for pm2.

8)	Run ` npm install -g pm2`.

9)	Close the terminal.
 
## Step four: Preparing to transfer the bot’s files over SFTP

We would have gone with FTP (File Transfer Protocol) but I was advised to use SFTP (Secure File Transfer Protocol) so here we are
(oh and using SFTP saves us a few steps).

1)	Download and install PuTTYgen [32-bit](https://the.earth.li/~sgtatham/putty/latest/w32/puttygen.exe) or [64-bit](https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe)

2)	Locate and open PuTTYgen.

![image](https://user-images.githubusercontent.com/49169805/87231702-486dbb00-c3b1-11ea-839c-c7728c44b061.png)

3)	Once the window has opened, click on generate (no need to change any settings) and move your mouse around to generate
_some randomness_. 

![image](https://user-images.githubusercontent.com/49169805/87231709-591e3100-c3b1-11ea-804c-a151ff298100.png)

4)	Change the key comment to **the output of the whoami command you ran earlier!!!**.
![image](https://user-images.githubusercontent.com/49169805/87231737-9f739000-c3b1-11ea-8ab9-e02e32c943b4.png)

5)	Save the private key. Keep it safe as you’ll need it later!

6)	Copy the public key.

a.	I would also recommend that you save the public key as well, anything could happen!

## Step five: Adding the key to Google Cloud Platform
1)	Click on the name of your compute engine.

2)	Click edit at the top.

![image](https://mesub.is-ne.at/899nJz.png)

3)	Scroll down to the SSH section and click “Show and edit”.

![image](https://mesub.is-ne.at/8rp9VM.png)

4)	Paste the **public** key that you copied from PuTTY earlier.

5)	Scroll down to the bottom and click save.

![image](https://mesub.is-ne.at/13KGYX.png)

## Step six: Uploading the files to Google Cloud Platform
Now we need to upload the bot’s files!

1)	Download and install the FileZilla **Client** [here.](https://filezilla-project.org/download.php?type=client)

2)	Run the application.

3)	Go to the settings menu.
![image](https://user-images.githubusercontent.com/49169805/87231818-393b3d00-c3b2-11ea-8b5a-efaea59df5fe.png)

4)	Go to the SFTP section.

5)	Click “Add key file” and add the **private** key file that you generated earlier.

6)	Press ok.

![image](https://mesub.is-ne.at/xqbyg8.png)

7)	Navigate to File > Site Manager.
![image](https://mesub.is-ne.at/NYgGfw.png)

8)	Click new site.

9)	Give it a name.

10)	Copy the external IP address from Google Cloud and place it in the host section.

11)	In the username section, put in the output of the `whoami` command (which you should have got from the terminal).

12)	Click ok to save it. (Password Prompt is optional)
![image](https://mesub.is-ne.at/OfBViJ.png)

## Step seven: Accessing the server
Now we’ve set it all up, we should be able to access the server!

1)	 Navigate to File > site manager.

![image](https://mesub.is-ne.at/NYgGfw.png)

2)	Click connect.

3)	If given a password prompt, just leave it blank and press enter.

4)	If given an unknown key prompt comes up, then click ok (Always trusting the host is up to you).
![image](https://mesub.is-ne.at/aCPwlv.png)

If it all works, then it should look like this:
![image](https://mesub.is-ne.at/GLe5bZ.png)

## Step eight: Transferring the bot’s files

1)	Locate your bot’s files in the internal navigator. 

![image](https://mesub.is-ne.at/Ye5tRh.png)

2)	Drag the files over.

3)	If it was successful then you should see the folder on the other side. ![image](https://mesub.is-ne.at/w6L4HW.png)

## Step nine: Inviting the bot to the server
Now it is time to get the bot in your servers!

1)	Go back to the discord developer page that you should have left open.

2)	Go to the hamburger menu and click on oauth2.

![image](https://mesub.is-ne.at/g4n1YW.png)

3)	Scroll down and select bot.

4)	Add the following permissions: manage channels, manage messages and attach files.
![image](https://mesub.is-ne.at/WZwOGJ.png)

5)	Navigate to the link generated.

6)	Add the bot to your sever(s).

## Step ten: Running the bot
Now that we’ve set everything up, we should be able to start the bot!

1)	Navigate to the bot’s directory (you should know your way around navigation commands in linux).

2)	Run `npm ci`.

3)	Run `npm start`.
If it all works then it should say "Done! Now listening to DMs" once it has loaded the config.

4)	Escape npm by pressing `control` + `c`.

5)	Run `pm2 start modmailbot-pm2.json` - This will ensure that the bot stays online when we close the terminal.
Congratulations! You’ve now got a modmail bot that is being run for free and you’ve gained a little Linux knowledge 😊.

# Terminal

Skip this section if you have followed the guide all the way up to here. Here's a [jumplink.](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#logs)

This allows you to download and configure the bot from the terminal, no fuss involved. I will not be providing support on this section. 
(Many thanks to [@dopeghoti](https://github.com/dopeghoti) for providing this method.)

Prerequisite: Please follow steps [1](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#step-one-discord-developer-portal) and [3](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#step-three-google-cloud-platform-nodejs-and-pm2) before continuing this section.

1) Make a directory and navigate to it.

2) Install wget (`sudo apt install wget`)

3) Run `wget https://github.com/Dragory/modmailbot/archive/v3.0.3.tar.gz`

4) Run `tar zxf v3.0.3.tar.gz`

5) Navigate to the directory it creates.

6) Run `cp config.{example.,}ini`

7) Run `nano config.ini` and edit the config to your liking.

Continue following the guide from step [9.](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#step-nine-inviting-the-bot-to-the-server)

# Logs

We've got the bot working now but you may want to have working logs. If this is the case then follow the instructions below.

Prequisite: Port 8890 (TCP) has to be opened. If you don't know how to open a port then check [PortForward's website](http://portforward.com/).

Once you've done that then:

1) Go to https://console.cloud.google.com/. 

2) Click on compute engine and then VM instances.

3) If you didn't dismiss the related actions section, there should be an option to go to the "firewall rules" section. Click it.
![image](https://mesub.is-ne.at/hjenkB.png)

 a) If there isn't anything about that then navigate to: https://console.cloud.google.com/networking/firewalls/.
 
You should be here:

![image](https://mesub.is-ne.at/aC1wLb.png)

4) Click on "Create firewall rule".

5) It should have the following details:
 **Name:** anything you want.

 Leave priority.

 **Direction:** Ingress. We are connecting to the server.

 **Action on match:** Allow.

 **Targets:** All instances in the network. You can use the other ones if you know what you're doing.

 **Source Filter:** IP ranges.

 **Source IP ranges:** `0.0.0.0/0`.

 **Protocols and ports**: Allow specific protocols and ports. tcp: `8890`.
 
 It should look a bit like this: ![image](https://mesub.is-ne.at/OHQO1j.png)
 
 6) Click create.
 
 7) Try it out! If you've set up everything correctly then it should work.
 
 You've now got a fully functioning modmail bot! Thank me later.
 
 # FAQS and issues
 
**It says "please run npm install before running the bot", what should I do?**
 
Don't do it. Verify that npm, node and nvm are all installed correctly and make sure that the bot's files are in the right directory (refer to Step three, instruction eight). If you are **very, very sure** that this is all correct then refer to the username issue [here](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#user-accounts). 
 
**Can I get the log links to use my website?**
 
Yes but I don't have a domain and this is out of scope for this guide. I can only say that you need to have a webserver and you also need to set up a reverse proxy.

**Can I have more than one installation running at the same time**

Yes. Just ensure that you set a different port and the second installation is in a different folder. You can also use environment variables.

**How long is this free for?**

Until Google decides to change its always free tier to something that isn't usable (unlikely).

**I get an error about gateway intents or the bot is stuck on "Connecting to Discord..."**

Make sure that the **server members intent** is enabled.

# User accounts

_This only applies if you are configuring the bot from your system and not doing it all from the terminal_

It is quite important that the username/key comment matches what the `whoami` command outputs, If this doesn't happen then you will either be unable to connect to the server OR your files will be on another user account (which you specified). 

In order to avoid this, **please please please** try to make sure that the username/key-comment matches the output of the `whoami` command. At the very minimum though (or if you aren't using PuTTY), **make sure that the key comment and the username for FileZilla match.** We can change the Linux username (on Google's end) by doing the following:

**IMPORTANT:** Files on your old username will be wiped, be sure that you don't need anything from it. You have been warned. 

1) Connect to your instance.

2) Click on the gear.

![image](https://mesub.is-ne.at/8oHUyO.png)

3) Click _Change Linux username._

![image](https://mesub.is-ne.at/thPt8R.png)

4) Enter the username that the key comment/FileZilla username used.

5) Click change.

You will be connected to the instance under the new username. The files that you transferred over should be there.

**Note:** It is very very likely that you will need to install all of the [necessary software again](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#nvm-nodejs-and-pm2) (it seems to install on a per user basis).

Of course you can easily avoid this by [configuring from the terminal](https://github.com/mesub7/modmailbot-community-resources/blob/master/#terminal).

# Support and feedback

If you are having issues with anything in this guide and you've tried to help yourself, then I am in [Dragory's discord server](https://discord.gg/vRuhG9R) (mesub#0556). I may not be online when you post your question though.

Additionally, if you've got any suggestions on how to improve the guide then please feel free to create an issue or find me in [Dragory's discord server](https://discord.gg/vRuhG9R).



_This guide was last updated on the 8/11/2020._
Guide revision: 5 (V1.4.1)
