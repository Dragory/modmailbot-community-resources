# Thread Permissions

If deployed in a single-server setup you must ensure that threads are created in a Category that only allows visibility to your staff.  

The below settings can be used in two-server setups as well.

1. Set [`inboxServerPermission`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#inboxserverpermission) to the ID of a role you give the people you want able to run ModMailBot commands.  For example, if you have some moderators that all have a "Moderator" role, get the ID of that role and set `inboxServerPermission` to it.    
    **Example:** `inboxServerPermission = 12345`    
    
    You can also set it to the name of a role ability if you don't want to create a bespoke role.  For example, if you want only people who have the ability to remove messages in your Discord server to issue commands to your bot, set it appropriately.     
    **Example:** `inboxServerPermission = manageMessages`
1. [Create a Category in your server.](https://support.discord.com/hc/en-us/articles/115001580171-Channel-Categories-101)
2. Set the permissions on that category such that only your staff can see the channels.  This is the same as setting permissions on channels, except in the Category.  You can use the role you created in step #1.
3. Create your Log Channel there in that category ( [`logChannelId`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#logchannelid) ).    
    **Example:** `logChannelId = 12345`
4. (optional) Create your Attachments channel ( [`attachmentStorageChannelId`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#attachmentstoragechannelid) ) in that category too, if you choose to use [`attachmentStorage = discord`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#attachmentstorage)    
    **Example:** `attachmentStorageChannelId = 12345`
5. Set [`categoryAutomation.newThread`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#categoryautomationnewthread) to the ID of that category.    
    **Example:** `categoryAutomation.newThread = 12345`
6. (optional) Consider setting [`anonymizeChannelName = on`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#anonymizechannelname).    

    Modified Discord clients can be configured to show channels and categories that the user shouldn't be able to see.  Using this method a user could see who is opening threads with your Modmail.  This is limited soley to the name of the channel/category and it's description; the contents of the channel are still private.
7. If your bot is currently running, restart it.
