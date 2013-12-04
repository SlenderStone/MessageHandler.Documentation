## Generate Alert

Generates an alert entity when a simple trigger is matched.

### Handler properties

**Trigger:** A condition that will trigger the handler to create an Alert entity, the trigger has to be created using Javascript syntax and must evaluate to a bool. You can access the message under consideration using the `msg` parameter. For example:

	msg.Temperature > 50

**Alert Type:** A string representing the type of alert to generate for the specified condition, some suggestions: "Warning", "Error", "Critical"

### Message requirements

**Serializer:** JSON

**Format:** Any

### Generated message

**Serializer:** JSON

**Format:**

	{
		Type: string,
        Trigger: string,
        Message: { ... },
		Who: string,
        When: datetime,
        Where: { Lat: double, Lon: double }        
	}

### Notes

**datetime:** [ISO 8601 standard ](http://en.wikipedia.org/wiki/ISO_8601), f.e. 2012-03-19T07:22Z