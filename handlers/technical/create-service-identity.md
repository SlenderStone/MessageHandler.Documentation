# Create service identity

Creates a service identity in Windows Azure Access Control service, based on an input name.

## Handler properties

**ServiceNamespace:** your ACS service namespace.

**ServiceNamespaceManagementUserName:** The username of an identity that is allowed to manage your ACS namespace

**ServiceNamespaceManagementUserKey:** The key of an identity that is allowed to manage your ACS namespace

**RelyingPartyName:** The relying party name for the identity to create

**RedirectUriPrefix:** The redirect uri prefix for the identity to create

**ValidityPeriod:** Number of days the generated key will be valid for the identity to create

## Message requirements

**Serializer:** JSON

**Format:** 

	{
		$type: MessageHandler.Contracts.AccountCreated.IAccountCreated,
		Name : string,
		Who: string,
		When: datetime,
		Where: { Lat: double, Lon: double }
	}

## Notes

**$type:** Type requirements will be removed in the future

**datetime:** [ISO 8601 standard ](http://en.wikipedia.org/wiki/ISO_8601), f.e. 2012-03-19T07:22Z