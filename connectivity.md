# Connectivity

Messages are pieces of data that travel from an origin to a destination. In many cases the origin and destination are somewhere in the physical or virtual world, presenting the need to route messages in and out of your channels in order to process them.

Setting up connectivity can be done in different ways.

 * Either an outside system pushes the messages actively into your channels, and also pulls them actively from, by using [endpoints](/documentation/connectivity/endpoints) on our gateway infrastructure. This approach is typically taken for external systems that are under your (or your partners) control, such as iot devices, mobile phones, websites, backend systems and so forth.
 * Alternatively handlers hosted inside your channels can actively pull the messages from the external system, so called [streams](/documentation/connectivity/streams), and push them back actively as well. This approach is most common when the external system is not under control. Typically the case when working with third party vendors and SAAS services.

 