## Persist entity to table storage

Persists an entity root / aggregate to table storage, where the natural identity is used as partitionkey. The intention is to have 1 partition per entity/aggregate and using the rows for other parts of the aggregate.

### Handler properties

**ConnectionString:** The connections string to the storage account where you want to persist the entities.

**DataType:** The name of the datatype, will serve as tablename for table where you want to store the entities as well

**PartitionKeyProperty:** The property name for which the value will be used as partitionkey for the entities and their children.

**RowKeyProperty:** (optional) The property name for which the value will be used as rowkey

### Message requirements

**Serializer:** JSON

**Format:** 

	{
		$type: MessageHandler.Contracts.IEntityCreated, MessageHandler.Contracts.EntityCreated,
		Entity : { ... },
		Who: string,
		When: datetime,
		Where: { Lat: double, Lon: double }
	}

### Outgoing message format

**Serializer:** JSON

**Format:** 

	{
		$type: MessageHandler.Contracts.IEntityPersisted, MessageHandler.Contracts.EntityPersisted,
		Entity : { ... },
		Who: string,
		When: datetime,
		Where: { Lat: double, Lon: double }
	}

### Notes

**$type:** Type requirements will be removed in the future

**datetime:** [ISO 8601 standard ](httcd\p://en.wikipedia.org/wiki/ISO_8601), f.e. 2012-03-19T07:22Z 