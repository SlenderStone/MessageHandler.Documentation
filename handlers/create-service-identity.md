## Create service identity

Creates a service identity in Windows Azure Access Control service, based on an input name.

### Message requirements

**Serializer:** JSON

**Format:** 

	{
		$type: MessageHandler.Contracts.AccountCreated.IAccountCreated,
		Name : string,
		Who: string,
		When: datetime,
		Where: { Lat: double, Lon: double }
	}

### Notes

**$type:** Type requirements will be removed in the future

**datetime:** [ISO 8601 standard ](http://en.wikipedia.org/wiki/ISO_8601), f.e. 2012-03-19T07:22Z