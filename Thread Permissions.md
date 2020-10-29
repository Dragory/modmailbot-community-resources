# Thread Permissions

If deployed in a single-server setup you must ensure that threads are created in a Category that only allows visibility to your staff.

1. [Create a Category in your server.](https://support.discord.com/hc/en-us/articles/115001580171-Channel-Categories-101)
2. Set the permissions on that category such that only your staff can see the channels.  This is the same as setting permissions on channels, except in the Category.
3. Create your Log Channel there ( `logChannelId` )
4. (optional) Create your Attachments channel ( `attachmentStorageChannelId` ) in there, if you choose to use [`attachmentStorage = discord`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#attachmentstorage)
5. Set [`categoryAutomation.newThread`](https://github.com/Dragory/modmailbot/blob/master/docs/configuration.md#categoryautomationnewthread) to that category.
6. If your bot is currently running, restart it.

**PLEASE NOTE:** with modified Discord clients users are able to see channel names even if your permissions don't allow them to see the channel.  
