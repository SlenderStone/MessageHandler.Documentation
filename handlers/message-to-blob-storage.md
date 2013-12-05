## Message to blob storage

Persists a message in a blob in blob storage. Messages will be appended to a blob on a per thread per day basis.

### Handler properties

**ConnectionString:** The connections string to the storage account where you want to persist the messages.

**Container:** The storage container where the files should be added to.

### Message requirements

**Serializer:** Any

**Format:** Any