# How to host your modmail bot on Oracle Cloud for free | Made by [@mesub7](https://github.com/mesub7)

Want to host your modmail bot for free? Don’t want to be ridiculed for using Heroku, Glitch or repl.it? This guide will take you through how to get the bot running on Oracle Cloud.

Following the success of my Google Cloud Platform Guide, I've deciced to make one for Oracle Cloud as well.
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

## Step three: Oracle Cloud, nodeJS & PM2
This is where we'll be hosting the bot.
1) Navigate to https://oracle.com/cloud/free and click 'Start for free'
