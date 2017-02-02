# Dynamic Json

The default serialization format for typed messages is JSON. 

The MessageHandler runtime also offers support to work with JSON objects in a dynamic fashion, in case the type of the message is not known. This document describes how dynamic JSON support can be used.

In order to turn a JSON string into a dynamic object, call `Decode` without type specification and assign the result to a dynamic variable.

<!-- start of code block -->

	var json = "{\"SomeProperty\":\"test\"}";
	dynamic deserialized = Json.Decode(json);

<!-- end of code block -->

Depending whether the json string represents an object or an array, the infrastructure will return an instance of DynamicJsonObject or DynamicJsonArray.


## Dynamic Json Object

The DynamicJsonObject allows to dynamically evaluate property names, so called `DuckTyping`.

<!-- start of code block -->

	var value = deserialized.SomeProperty;

<!-- end of code block -->

It also allows to evaluate properties using a dictioanry notation.

<!-- start of code block -->

	var value = deserialized["SomeProperty"];

<!-- end of code block -->

Note: there is a known memory problem in the dynamic callsite generation logic when using this method, it's advised to first cast the dynamic object to a `Dictionary<string, object>` before applying this method.

### Supported Member Types

**Primitives:** String, Int, Double.

**DateTimeOffset:** Dates can be either represented as Iso8601 string values `1997-07-16T19:20:30.45+01:00`, or using the Microsoft proprietary format `/(19347598357)/`. The deserialization infrastructure will return `DateTimeOffset` instances to ensure proper timezone handling (in contrast to `DateTime` instances). All Iso8601 strings that represent UTC times (Z and +/-offset notations will be converted to the UTC timezone), others will be returned as local time.

**References:** All child objects and arrays will be recursively deserialized.

### Casting

It is possible to cast a DynamicJsonObject to a typed instance by simply assigning it to a variable of that type.

<!-- start of code block -->

	SerializedObject casted = deserialized;
	var value = casted.SomeProperty;

<!-- end of code block -->

It is also possible to cast this object to a Dictionary<string, object> and use the properties using a dictionary entry notation.

<!-- start of code block -->

	Dictionary<string, object> casted = deserialized;
	var value = casted["SomeProperty"];

<!-- end of code block -->

## Dynamic Json Array

The DynamicJsonArray allows to iterate through a list of DynamicJsonObjects.

<!-- start of code block -->

	dynamic deserialized = Json.Decode(json);
	foreach (var item in deserialized)
	{
		item.SomeProperty = "test";
	}
<!-- end of code block -->

Just like any other array, objects can also be accessed by index.

<!-- start of code block -->

	dynamic deserialized = Json.Decode(json);
	deserialized[0].SomeProperty = "test";
	deserialized[1].SomeProperty = "test";

<!-- end of code block -->

### Casting

It is possible to cast a DynamicJsonArray to a typed instance of the array.

<!-- start of code block -->

	SerializedObject[] casted = deserialized;

<!-- end of code block -->

It is also possible to cast it to an object[]

<!-- start of code block -->

	SerializedObject[] casted = deserialized;

<!-- end of code block -->

Or an Array object

<!-- start of code block -->

	Array casted = deserialized;

<!-- end of code block -->