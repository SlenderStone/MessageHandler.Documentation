## Persist claimset to table storage

Persists a claimset returned from Windows Azure Access Control Service to table storage.

### Handler properties

**ConnectionString:** The connections string to the storage account where you want to persist the claimsets.

**TableName:** The name of the table where you want to store the claimsets

**PartitionKeyClaim:** The claim identifier for which the value will be used as partitionkey for all claims

**RowKeyClaim:** The claim identifier for which the value will be used as rowkey


### Message requirements

**Serializer:** JSON

**Format:** 

	{
		$type: MessageHandler.Contracts.IUserLoggedIn,
		Claimset : { "claim":"value", "claim2":"value2"},
		Who: string,
		When: datetime,
		Where: { Lat: double, Lon: double }
	}

### Notes

**$type:** Type requirements will be removed in the future

**datetime:** [ISO 8601 standard ](http://en.wikipedia.org/wiki/ISO_8601), f.e. 2012-03-19T07:22Z