# Connectivity

Messages are pieces of data that travel from an origin to a destination. In many cases the origin and destination are somewhere in the physical or virtual world, presenting the need to route messages in and out of your channels in order to process them.

Setting up this type of connectivity can be done in 2 different ways, each with it's own particular characteristics.

 * [Endpoints](/documentation/connectivity/endpoints): One way of setting up connectivity, is to let an outside system push messages actively into your channels, and also pull them actively from our system, by using [endpoints](/documentation/connectivity/endpoints) on our gateway infrastructure. This approach is typically taken for external systems that are under your (or your partners) control, such as iot devices, mobile phones, websites, backend systems and so forth.
 * [Handlers](/documentation/handlers): Alternatively handlers hosted inside your channels can actively pull the messages from the external system, using so called [streams](/documentation/connectivity/streams), and push them back actively as well. This approach is most common when the external system is not under control. Typically the case when working with third party vendors and SAAS services.

 