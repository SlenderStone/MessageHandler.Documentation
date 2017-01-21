# Json

The default serialization format for typed messages is JSON. 

The MessageHandler runtime also offers support to work with JSON objects in a dynamic fashion, in case the type of the message is not known. This document describes how dynamic JSON support can be used.

In order to turn a JSON string into a dynamic object, call `Decode` without type specification and assign the result to a dynamic variable.

```
var json = "{\"SomeProperty\":\"test\"}";

dynamic deserialized = Json.Decode(json);
```

Depending whether the json string represents an object or an array, the infrastructure will return an instance of DynamicJsonObject or DynamicJsonArray.


## Dynamic Json Object

The DynamicJsonObject allows to dynamically evaluate property names, so called `DuckTyping`.

```
var value = deserialized.SomeProperty;
```

It also allows to evaluate properties using a dictioanry notation.

```
var value = deserialized["SomeProperty"];
```

Note: there is a known memory problem in the dynamic callsite generation logic when using this method, it's advised to first cast the dynamic object to a `Dictionary<string, object>` before applying this method.

### Supported Member Types

**Primitives:** String, Int, Double.

**DateTimeOffset:** Dates can be either represented as Iso8601 string values `1997-07-16T19:20:30.45+01:00`, or using the Microsoft proprietary format `/(19347598357)/`. The deserialization infrastructure will return `DateTimeOffset` instances to ensure proper timezone handling (in contrast to `DateTime` instances). All Iso8601 strings that represent UTC times (Z and +/-offset notations will be converted to the UTC timezone), others will be returned as local time.

**References:** All child objects and arrays will be recursively deserialized.

### Casting

It is possible to cast a DynamicJsonObject to a typed instance by simply assigning it to a variable of that type.

```
SerializedObject casted = deserialized;

var value = casted.SomeProperty;
```

It is also possible to cast this object to a Dictionary<string, object> and use the properties using a dictionary entry notation.

```
Dictionary<string, object> casted = deserialized;

var value = casted["SomeProperty"];
```

## Dynamic Json Array

The DynamicJsonArray allows to iterate through a list of DynamicJsonObjects.

```
 dynamic deserialized = Json.Decode(json);

 foreach (var item in deserialized)
 {
	item.SomeProperty = "test";
 }
```

Just like any other array, objects can also be accessed by index.

```
 dynamic deserialized = Json.Decode(json);

 deserialized[0].SomeProperty = "test";
 deserialized[1].SomeProperty = "test";
```

### Casting

It is possible to cast a DynamicJsonArray to a typed instance of the array.

```
SerializedObject[] casted = deserialized;
```

It is also possible to cast it to an object[]

```
SerializedObject[] casted = deserialized;
```

Or an Array object

```
Array casted = deserialized;
```