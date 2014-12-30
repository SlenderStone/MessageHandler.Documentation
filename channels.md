# Channels

Channels are the core unit in MessageHandler. In essence they represent the flow that messages follow while travelling through our system. They define how messages get routed between our processing units, called handlers, and how all the pieces are wired together. Therefore we like to think of channels as the core value units in our system, each one represents a solution to a specific business problem.

Almost every administrative action taken inside MessageHandler is performed at the channel level: from configuration to deployment, to monitoring and sharing. If you want to get the most value out of MessageHandler it is important to learn:

 * [How channels operate internally](/documentation/designing-channels/channel-internals)
 * [How you can design channels yourself](/documentation/designing-channels)
 
But of course you can also get started more quickly than designing one yourself, our gallery already has a few recipes for channels ready for you.

# Existing channels in our gallery

 * [Temperature Monitor](/documentation/channels/temperature-monitor)
 * [Twitter Engagement Analysis](/documentation/channels/twitter-engagement-analysis)
 * [Light Detection](/documentation/channels/light-detection)
 * [Myget Build Automation](/documentation/channels/myget-build-automation)