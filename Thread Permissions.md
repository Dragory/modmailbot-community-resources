# Thread Permissions

If deployed in a single-server setup you must ensure that threads are created in a Category that only allows visibility to your staff.

1. [Create a Category in your server.](https://support.discord.com/hc/en-us/articles/115001580171-Channel-Categories-101)
2. Set the permissions on that category such that only your staff can see the channels.  This is the same as setting permissions on channels, except in the Category.
3. Create your Log Channel there ( `logChannelId` )
4. (optional) Create your Attachments channel ( `attachmentStorageChannelId` ) in there, if you choose to use [`attachmentStorage = discord`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#attachmentstorage)
5. Set [`categoryAutomation.newThread`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#categoryautomationnewthread) to that category.
6. (optional) Consider setting [`anonymizeChannelName = on`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#anonymizechannelname).  Modified Discord clients can be configured to show channels and categories that the user shouldn't be able to see.  Using this method a user could see who is opening threads with your Modmail.  This is limited soley to the name of the channel/category and it's description; the contents of the channel are still private.
7. If your bot is currently running, restart it.
