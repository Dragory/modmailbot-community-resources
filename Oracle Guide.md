# How to host your modmail bot on Oracle Cloud for free | Made by [@mesub7](https://github.com/mesub7)

Want to host your modmail bot for free? Don’t want to be ridiculed for using Heroku, Glitch or repl.it? This guide will take you through how to get the bot running on Oracle Cloud.

Following the success of my Google Cloud Platform Guide, I've decided to make one for Oracle Cloud as well.
I would strongly recommend that you use [Google Cloud Platform](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md) as the set-up is much less confusing and there's added benefits to using it (Google brand recognition, an app that you can use to SSH into etc). However, you may pick Oracle because you plan to run multiple bots [(which google can [eventually] charge for)](https://www.reddit.com/r/googlecloud/comments/lxjf07/getting_charged_for_egress_under_1gbmonth/) or you just don't want to use Google.

**DISCLAIMER: I am not responsible for any charges that you face due to you digressing from the guide.**

**(Need to update your installation? Check [this.](https://github.com/Dragory/modmailbot/blob/master/docs/updating.md))**

**Highly recommended: Configure from the host to save time and to minimise errors**
If you know your way a little bit around Linux and want to just download and configure the bot remotely, you can use the Terminal instructions found [here](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#terminal). It's quicker but can be confusing if you're new to server administration as there is no graphical interface (buttons to press).

Due to the multiple services/applications that we are using, this guide may be outdated. If this is the case then please let me know here or in the discord server.
I am aware that there are a few alternative methods to this. If you know what you are doing, then by all means use part of this guide and your own method. Just note that if you do this, I can’t really provide support as your method may be the reason it is not working.

This guide presumes that you have basic terminal/command prompt knowledge and you are running Windows 10. If you don’t know your `dir` from your `cd` then it might be worth getting somebody else (not me) set up this for you! Google has all the information that you need. I cannot be sure if the steps are different in other operating systems, use your own judgment for equivalents. 

So, with that out of the way, let us begin!

## Step one: Discord Developer Portal
1)	Navigate to https://discord.com/developers/applications and click new application.
2)	Give it a name.
3)	Go to the bot section (which is found on the left-hand navigation pane) and click add bot.
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
4)	Open the config.ini file and edit it to your liking. You’ll need the bot token from the developer portal as well as a `mainServerId` and an `inboxServerId`.
For information on what can go in the config file, check [the official documentation.](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md)
![image](https://user-images.githubusercontent.com/49169805/87231371-9a611180-c3ae-11ea-8a1b-b560e9a4f08a.png)
5)	Save the config file.
6)	Make sure you know where you’ve kept this folder, you will need it later on.



## Step three: Oracle Cloud & PuTTY
This is where we'll be hosting the bot.
1) Navigate to https://oracle.com/cloud/free and click 'Start for free'.
2) Enter your details and verify your email.
3) Continue entering your details until you get to pick your home region and set it to **US East (Ashburn)** (This is the closest to GCP's us-east1-b, which Discord uses for its servers). **You cannot change your home region once you've signed up**.

![image](https://mesub.is-ne.at/565n2fNCl.png)

4) Enter your address and billing information.

_Hold on. I thought it was free?_
It is free, but for verification purposes, Oracle needs it. You will not be charged unless you explicitly click upgrade.

_But I [insert excuse here] and don’t have a card!_
I figured as such. Below, there is a flowchart that goes through what processes I think is feasible.
**Oracle only seems to accept debit/credit cards. If you can't get hold of one then sadly you're out of luck.**

![image](https://user-images.githubusercontent.com/49169805/128049925-50b64bc8-159d-4369-b93f-ac4147763967.png)

5) Accept the agreement and click 'Start my free trial'.

Once it's all done, you should see something like this: ![image](https://mesub.is-ne.at/565xg3tEr.png)

If you've been checking your emails, you'll find that we need a wait a few minutes for Oracle to fully set up our account on their end. We'll use this waiting period to download PuTTY. Keep the Oracle tab open though.

6) Go to [PuTTY's website](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) and get the latest packaged files for your system architecture (32-bit or 64 bit).

7)	Locate and open PuTTYgen.

![image](https://user-images.githubusercontent.com/49169805/87231702-486dbb00-c3b1-11ea-839c-c7728c44b061.png)

8)	Once the window has opened, click on generate (no need to change any settings, this includes the key comment) and move your mouse around to generate
_some randomness_. 

![image](https://user-images.githubusercontent.com/49169805/87231709-591e3100-c3b1-11ea-804c-a151ff298100.png)


9)	Save the private key. Keep it safe as you’ll need it later!

10)	Copy the public key.

a.	Select all of the generated key that appears under Public key for pasting into **OpenSSH authorized_keys file** and copy it.
b.	Paste it into a text file, and then save the file in the same location as the private key.

(Do not use Save public key because it does not save the key in the OpenSSH format, which is vital for later.)

By the time this is done, you should have hopefully recieved an email saying that your account is fully provisioned, so we can create the instance.

8) Return to the Oracle tab, scroll down and click 'Create a VM instance'.
9) Give your VM a name.
10) Go to images, click edit and select an **Always Free Eligible** image of your choice from **Platform images**: ![image](https://mesub.is-ne.at/565M_kjdY.png)
11) I will use Unbuntu as I've got the most familiarity with it.
12) Edit the settings accordingly to ensure that **Availability Domain and Shape** have Always Free eligible next to them.
13) In the networking section: ensure that 'Assign a public IPv4 address' is set to yes. If not, change it. ![image](https://mesub.is-ne.at/565OnFcsh.png)
14) In the 'Add SSH keys' section, either upload your saved **public** key or paste it from PuTTY. ![image](https://mesub.is-ne.at/565PiHXqD.png)
15) Review your settings and click create!

Your new instance has been created!

Once it's been provisioned, it'll look something like this:
![image](https://mesub.is-ne.at/565QWKvQV.png)

We can then connect to it with an SSH client, since Oracle doesn't offer SSH via the browser.

16) Open your SSH client of choice (I choose PuTTY since it's already installed).
17) On the side pane, click the + by SSH and then click Auth (not the + though) ![image](https://mesub.is-ne.at/565Vfk35Y.png)
18) Click 'Browse' by 'Private key for authentication' and select the **private key** ![image](https://mesub.is-ne.at/565VF98oB.png)
19) Scroll back up the side pane and click session (not the -) ![image](https://mesub.is-ne.at/565WnpJBM.png)
20) Paste the public IP adress (which you'll find on the instance page) into the IP address field, click Default Settings, click save and then click Open. (This ensures that the IP adress is saved for next time you open the PuTTY) 
a.	If any prompt comes up about the host's fingerprint/connecting to it, click yes.
21) Enter your username. If you are using **Ubuntu**, your username is: `ubuntu`, otherwise it is `opc`.
22) If it all worked successfully, then you should get something like this: ![image](https://mesub.is-ne.at/565WH70hz.png)

Now we can install the necessary software needed.

## Step four: NodeJS & PM2
1)	Navigate to https://github.com/nvm-sh/nvm#installing-and-updating

2)	Copy the first `curl -o-…` command (from the page). This will run the install script (doing all the dirty work for us). :P

3)	Close and reopen your terminal (the window).

4)	Run `nvm` to verify that it has been installed.

Now to install node.

5)	List the versions of node available by running `nvm ls-remote`.

Now (at the time of writing) since the bot supports versions 12 to 14, for compatibility's sake, we'll install the latest version of V14 (feel free to install any version between 12 and 14).

6)	Run `nvm install 12.19.0` (or whatever version you want that's between V12 and V14 (like 14.13.1 for example).

7)	Optional: run `nvm ls` to verify that it is installed.

Now for pm2.

8)	Run ` npm install -g pm2`.

9)	Close the terminal.

## Step five: Uploading the bot's files to Oracle Cloud
Now we need to upload the bot’s files!

You can use PSFTP or Filezilla (or something else) for this. I'm only going to cover FileZilla for this as it's the application which I'm most familiar with.

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

10)	Copy the external IP address from Oracle and place it in the host section.

11)	In the username section, put in `ubuntu` or `opc`, depending on if you use ubuntu or not.

12)	Click ok to save it. (Password Prompt is optional)
![image](https://mesub.is-ne.at/5664pFGaL.png)

## Step five: Accessing the server
Now we’ve set it all up, we should be able to access the server!

1)	 Navigate to File > site manager.

![image](https://mesub.is-ne.at/NYgGfw.png)

2)	Click connect.

3)	If given a password prompt, just leave it blank and press enter.

4)	If given an unknown key prompt comes up, then click ok (Always trusting the host is up to you).
![image](https://mesub.is-ne.at/aCPwlv.png)

If it all works, then it should look like this:
![image](https://mesub.is-ne.at/GLe5bZ.png)

## Step six: Transferring the bot’s files

1)	Locate your bot’s files in the internal navigator. 

![image](https://mesub.is-ne.at/Ye5tRh.png)

2)	Drag the files over.

3)	If it was successful then you should see the folder on the other side. ![image](https://mesub.is-ne.at/w6L4HW.png)

## Step seven: Inviting the bot to the server
Now it is time to get the bot in your servers!

1)	Go back to the discord developer page that you should have left open.

2)	Go to the hamburger menu and click on oauth2.

![image](https://mesub.is-ne.at/g4n1YW.png)

3)	Scroll down and select bot.

4)	Add the following permissions: manage channels, manage messages and attach files.
![image](https://mesub.is-ne.at/WZwOGJ.png)

5)	Navigate to the link generated.

6)	Add the bot to your sever(s).

## Step eight: Running the bot
Now that we’ve set everything up, we should be able to start the bot!

1)	Navigate to the bot’s directory (you should know your way around navigation commands in linux).

2)	Run `npm ci`.

3)	Run `npm start`.
If it all works then it should say "Done! Now listening to DMs" once it has loaded the config.

4)	Escape npm by pressing `control` + `c`.

5)	Run `pm2 start modmailbot-pm2.json` - This will ensure that the bot stays online when we close the terminal.
Congratulations! You’ve now got a modmail bot that is being run for free and you’ve gained a little Linux knowledge 😊.

# Terminal

Skip this section if you have followed the guide all the way up to here. Here's a [jumplink.](https://github.com/Dragory/modmailbot-community-resources/blob/master/Oracle%20Guide.md#logs)

This allows you to download and configure the bot from the terminal, no fuss involved. I will not be providing support on this section. 
(Many thanks to [@dopeghoti](https://github.com/dopeghoti) for providing this method.)

Prerequisite: Please follow steps [1](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#step-one-discord-developer-portal), [3](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md#step-three-google-cloud-platform-nodejs-and-pm2) and [4]() before continuing this section.

# Logs

blah

# Support and feedback

If you are having issues with anything in this guide and you've tried to help yourself, then I am in [Dragory's discord server](https://discord.gg/vRuhG9R) (mesub#0556). I may not be online when you post your question though.

Additionally, if you've got any suggestions on how to improve the guide then please feel free to create an issue or find me in [Dragory's discord server](https://discord.gg/vRuhG9R).



_This guide was last updated on the 3/08/2021._
Guide revision: 1 (V1.0.0)
