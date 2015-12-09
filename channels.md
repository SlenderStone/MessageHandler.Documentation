# Channels

Channels are the core unit in MessageHandler. In essence they represent a specific behaviour. They define the flow that messages follow while travelling through our system, how messages get routed between our processing units, and how all the pieces are wired together and how they are configured. Therefore we like to think of channels as the core of our system, each one represents a solution to a specific business problem.

Almost every administrative action taken inside MessageHandler is performed at the channel level: from configuration to deployment, to monitoring and sharing. If you want to get the most value out of MessageHandler it is important to learn:

 * [How channels operate internally](/documentation/channels/channel-internals)
 * [How you can design channels yourself](/documentation/channels/designing-channels)
 
But of course you can also get started more quickly, instead of designing one yourself you can grab one from our gallery, our gallery already has a few recipes for channels designed by fellow community members, and ready for you to use.

 * [Gallery](/gallery/channels)