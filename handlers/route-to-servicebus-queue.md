## Route to servicebus queue

Routes messages to specific service bus queues based on a condition map

### Handler properties

**Routes:** A dictionary, consisting of a trigger as key and a queue@namespace as value. The trigger has to be created using Javascript syntax and must evaluate to a bool. You can access the message under consideration using the `msg` parameter. For example:

	{ 
		'msg.Temperature > 50' : 'warningqueue@Endpoint=sb://{namespace}.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue={key}',
	  	'msg.Temperature > 90' : 'criticalqueue@Endpoint=sb://{namespace}.servicebus.windows.net/;SharedSecretIssuer=owner;SharedSecretValue={key}'
	}

### Message requirements

**Serializer:** JSON

**Format:** Any
