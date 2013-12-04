## Persist Entity To Sql

Persists an entity to sql server or sql azure.

### Handler properties

**ConnectionString:** The connectionstring to your sql server or sql azure instance.

**DataType:** The datatype for the entities to persist.

**Table:** The name of the table where the entities will be persisted.

**Map:** A javascript serialized dictionary of property name to column name mappings, example

	{'property1':'Column1', 'property2':'Column2'}

### Message requirements

**Serializer:** JSON

**Format:**

	{
		Type: string,
		Entity: { ... }
	}