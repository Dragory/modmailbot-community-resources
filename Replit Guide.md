# How to Host Dragony's ModMail Bot, on Replit, for Free!
You're considering to host the bot, but you have no money. This is a guide to how you can run Dragony's ModMail bot on Replit, for free. As Replit is classed as a nonstandard host, according to the ModMail staff, you need to do a lot of tweaking to get it working. This guide is an easy way to follow those steps and get your bot up and running! If you're not feeling Replit, there's another great guide made by [@mesub7](https://github.com/mesub7) that takes you through hosting the bot on [Google Cloud](https://github.com/Dragory/modmailbot-community-resources/blob/master/GCP%20Guide.md). 

# Main Instructions
## Step 1: Creating the Bot
1)	Go to: https://discord.com/developers/applications.
2)	Click 'New Application', give it a name and press 'Create'.
3)	On the left, press 'Bot' and in that menu, press 'Add Bot'. Click 'Yes, do it'.
4)	Turn on 'Server Members Intent'. This is necessary for the bots function. If you want plugins, you may need to turn on 'Presence Intent'.
5)	Press 'Copy' in the Token sub-menu. This will copy the bot's token for you to use later. **Do not share this with anybody.**
6)	Go into the 'OAuth2' menu on the left, just above 'Bot'. Then go into the 'URL Generator' menu.
7)	In the 'Scopes' sub-menu, check the 'bot' box. In the 'Bot Permissions' sub-menu, check these following boxes: `Manage Channels`, `Manage Messages` and `Attach Files`. You do not need to give the bot administrator permission or any other permissions.
8)	Underneath there is a 'Generated URL'. Copy that and go into a new tab on your browser, and invite the bot to the server(s) you want it to be in.

## Step 2: Downloading the Code
1)	Go to: https://github.com/Dragory/modmailbot/releases.
2)	Download the latest ZIP file. At the time of writing, this is v3.3.2.
3)	This should automatically download to your computer. Unzip the folder.
4)	Remember where the folder is located. You will need it later.

## Step 3: Signing into Replit
1)	Go to: https://replit.com/signup.
2)	Sign up to an account, or if you have one already, log into it.
3)	In the top left corner press 'Create Repl'. Make sure the programming language is set to Node.js.
4)	Give it a name to remember and press 'Create Repl' in the sub-menu that came up.

## Step 4: Importing the Code
1)	Go to your Repl. The link should be something like: https://replit.com/@(username)/(repl_name)/. 
2)	Right click the index.js file that is generated and press 'Delete. Press 'Yes, delete this file' in the sub-menu.
3)	At the top of the 'Files' tab press the ⋮ symbol. Press 'Upload Folder'.
4)	Double click the first folder and you should see a folder of the same name. Click that once and press 'Upload'.
5)	Back in your browser, allow all the files to be uploaded.
6)	Take all of the files and folders out of the main folder (drag all of them out to the bottom) and delete that folder.

## Step 5: Adding and Editing Necessary Files
1)	Right click the main folder and press 'Add File'.
2)	In the new file box, put **.replit** as the name.
3)	Copy this code into the new **.replit** file:
```
language = "nodejs"
run = "npm run start"
```
4)	Rename the file 'config.example.ini' to 'config.ini'
5)	Delete the line **token = REPLACE_WITH_TOKEN**. 
6)	Add your server IDs and channel IDs, where provided, to that file. If you need help with this, check out the [official Setup Guide](https://github.com/Dragory/modmailbot/blob/master/docs/setup.md) up to Step 4 / 5 of the Single / Two Server Setup sections.

## Step 6: Setting up the Token
1)	On the left of the Repl find the lock symbol. Click it.
2)	Press 'Got it' and then a small menu comes up.
3)	In the 'Key' box, type **MM_TOKEN**. In the 'Value' box, right click and Paste. If something irrelevant comes up, go back to Stage 5 on Step 1.

## Step 7: Creating a MySQL Database
1)	Go to: https://remotemysql.com/.
2)	At the top press 'Login' and in that menu press 'Create Account'.
3)	Enter your details to remember, complete the captcha and press 'Create Account'. Verify your email.
4)	Go to: https://remotemysql.com/databases.php?action=new. This creates a new database without needing to take the survey.
6)	Your Database Name, Username and Password will come up. Make sure to save these on a text document, as you'll need them later.

## Step 8: Logging in to your MySQL Database
1)	Go to: https://remotemysql.com/phpmyadmin/index.php.
2)	With the username and password you just saved on that text document, log in. Don't use your RemoteMySQL login you just signed up with. 
3)	On the left find the name of your database (it's the same as your username) and click it.
4)	On the top bar, find 'Operations' and click that. 
5)	Find the 'Collation' sub-menu at the bottom and click the dropdown box. Find this collation: **utf8mb4_general_ci**. 
6)	Check both 'Change all tables collations' and 'Change all tables columns collations' underneath and press 'Go'.

## Step 9: Linking your Bot to the MySQL Database
1)	Go back to your bot and go into the config.ini file.
2)	Add these lines to the bottom. Ensure that you fill it in with the values when needed:
```ini
dbType = mysql
mysqlOptions.host = remotemysql.com
mysqlOptions.database = YourDatabaseName
mysqlOptions.user = YourDatabaseUsername
```
3)	On the left of the Repl find the lock symbol. Click it.
4)	In the 'Key' box, type **MM_MYSQL_OPTIONS__PASSWORD**. Take note of the single underscore after MYSQL and the double one after OPTIONS.
5)	In the 'Value' box, put in your MySQL database password (not your RemoteMySQL password). 

## Step 10: Keeping the bot online 24 / 7
1)	On your Repl, press 'Run'. Wait for the bot to print 'Done! Now listening to DMs' in the Console.
2)	Above the Console area there should now be a web page, with a link present. This is embedded into the website. On some browsers, the contents of the web page does not actually show. Copy the link above this embedded website. It should look like: https://(repl_name).(username).repl.co.
3)	Go to: https://uptimerobot.com/.
4)	Click the button saying 'Register for FREE'.
5)	Enter your details to remember and press 'Register Now'. Verify your email.
6)	On the left, press the button 'Add New Monitor'.
7)	In this sub-menu, put the Monitor Type as HTTP(s). Add a name to remember, like 'ModMail bot' and enter the URL that you got on Step 2 in the URL box.
8)	Click 'Create Monitor' twice as it wants you to add an emergency contact. You can do this, but you will get spammed. The monitor will look like it is down constantly, that is because there's nothing on the web page. This is absolutely fine, so don't be worried if you see that your bot is down, because 99% of the time it isn't.

# Viewing Logs
Due to how the bot gets the link to logs, it does not work because it is taking the Replit server's IP address as it's own. You can do one of two options to make it work. Personally I prefer the second option as it allows you to view the log, yourself, in Discord. 

## Using the Default Option (logstorage = local)
1)  Go to your bot and go into the config.ini file.
2)  Put this in the new line. Ensure to fill in the values correctly:
```ini
url = https://(repl_name).(username).repl.co
```
3)  You should be done!

## Using the Attachments Option (logstorage = attachment)
1)  Make sure this line exists:
```ini
logstorage = attachment
```
2)  You can specify the directory where they are stored on Replit by doing:
```ini
logOptions.attachmentDirectory = (directory)
```
3)  You can allow threads that don't have an attachment to send a log link instead. You need to do Stage 2 of the Default Option for this to be configured correctly.
```ini
logOptions.allowAttachmentUrlFallback = on
```

# Extra Help
## Frequently Asked Questions
#### Why are you using MySQL, not the inbuilt SQLite?
- Due to how Replit operates, it resets the database to a previous version every few hours or so. The SQLite database is located directly on the Repl, but the MySQL database is external and therefore can't be rolledback. You could skip Steps 7-9 completely, but there are downsides if you have ModMail threads that last a long time, as the channels may be disconnected and you may lose logs.
#### Why do I need to make the Token and MySQL Password Environmental Variables?
- As the Repl is potentially in public view, people could simply go on the repl, find the config file and steal your token, and therefore hack the bot. Making it a secret means that Replit will not allow anybody else, but you or team members, to view the variables. You should also keep your MySQL password private because if someone gets hold of it, they can see everyone and everything that has ever been ModMailed, delete the table and database etc. 
#### I can't log in to phpMyAdmin / RemoteMySQL with the right details!
- There are different logins for RemoteMySQL and phpMyAdmin. RemoteMySQL is your Email and Password, that you initially signed up to the website with, to log in, create new databases and see how much storage space is taken up. However, the phpMyAdmin log in is not this. Instead, it is the random character username and password. This is most likely the issue you are facing if you cannot do this.
#### Can I initially start using Replit and then move elsewhere?
- Yes! As the database is on a separate website, you can simply link the database to your new project. You can simply re-download the bot itself from the [releases page](https://github.com/Dragory/modmailbot/releases) and then re-do Step 9. You may want to save your log files so you can view them.


## Getting Support
Like mentioned in the description at the beginning, Replit is classed as a nonstandard host, according to the ModMail staff. If you're having a problem with any aspect of this tutorial, you shouldn't go to them, as this is unofficial. I have tried to explain this in as much detail as possible in a simplistic way. 

If you're having issues with something else regarding the bot, you should be going to the official sources. This can be achieved by joining their [Discord Server](https://discord.gg/vRuhG9R) or by going to their [Issues Respository](https://github.com/Dragory/modmailbot/issues) and reporting the bugs.

My friend and co-writer **thejokeiscancelled** is on the Discord server and should be able to assist with any issues you may face about this guide or ModMail in general. 

I hope you can benefit from this guide.
-[Kyle Wordpress](https://github.com/kyle-wordpress)
