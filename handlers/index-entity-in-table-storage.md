## Index entity in table storage

Persists an entity root / aggregate to table storage, where a lookup key is used as partitionkey. The intention is to have 1 partition for every query you want to execute, consider these tables as secondary indexes with values copied over.

### Handler properties

**ConnectionString:** The connections string to the storage account where you want to persist the entities.

**DataType:** The name of the datatype, will serve as part of the tablename for the table where you want to store the entities as well, the tablename will be appended with "By" + `IndexByProperty`

**IndexByProperty:** The property name for which the value will be used as partitionkey for the entities and their children.

**OrderByProperty:** The property name for which the value will be used as rowkey, in this case to be used for determining order in the partition

### Message requirements

**Serializer:** JSON

**Format:** 

	{
		$type: MessageHandler.Contracts.IEntityPersisted, MessageHandler.Contracts.EntityPersisted,
		Entity : { ... },
		Who: string,
		When: datetime,
		Where: { Lat: double, Lon: double }
	}

### Outgoing message format

**Serializer:** JSON

**Format:** 

	{
		$type: MessageHandler.Contracts.IEntityIndexed, MessageHandler.Contracts.EntityIndexed,
		Entity : { ... },
		Who: string,
		When: datetime,
		Where: { Lat: double, Lon: double }
	}

### Notes

**$type:** Type requirements will be removed in the future

**datetime:** [ISO 8601 standard ](httcd\p://en.wikipedia.org/wiki/ISO_8601), f.e. 2012-03-19T07:22Z 