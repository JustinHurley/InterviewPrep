---
tags:
  - system_design
  - component
---
# Blob Store
A blob store is a storage solution for unstructured data. These will typically store photos, videos, audio, binary executables, and other non-document based or binary-data.
- All data is stored as a blob
- Blobs have no subdirectories or hierarchy 
- Big blobs may be split into smaller blobs called **chunks**
- E.g. S3 Buckets are used as blob stores
## When to Use
- When you are storing unconventional data (photos, videos, audio, .exe files)
- When you want to upload data once and read it many times
- If you are a streaming service
## Blob Store Design
### API Design
#### Create Container
Create a container to store blobs with `createContainer(containerName)`
#### Upload Blob
Upload the blob data to a given container, sending the blob data as binary information `putBlob(containerPath, blobName, data)`
#### Download Blob
Get the blob from the blob store `getBlob(blobPath)`
#### Delete Blob
Remove the blob from the store `deleteBlob(blobPath)`
### Components
- Client: the user
- Rate limiter: to keep requests made to service in check
- Load balancer: to route requests to the best blob store for that request/client
- Front-end servers: used to generate blob store requests and send them to the backend
- Data nodes: the blob stores
- Metadata nodes: where account data, permissions, blob IDs, container IDs and more 
- Monitoring service: monitors the health of other nodes in the system, and reports metrics and alarms if necessary
- Administrator: someone who resolves system issues and who may be alarmed in the event of an outage
### Design Considerations 
When creating and storing blobs, there are some abstractions we can make to simplify the blob storage process
1. **User account**: Users uniquely get identified on this layer through their `account_ID`. Blobs uploaded by users are maintained in their containers.
2. **Container**: Each user has a set of containers that are all uniquely identified by a `container_ID`. These containers contain blobs.
3. **Blob**: This layer contains information about blobs that are uniquely identified by their `blob_ID`. This layer maintains information about the metadata of blobs that’s vital for achieving the availability and reliability of the system.

| **Level** | **Uniquely identified by** | **Information** | **Sharded by** | **Mapping** |
| ---- | ---- | ---- | ---- | ---- |
| User’s blob store account | `account_ID` | list of `containers_ID` values | `account_ID` | Account -> list of containers |
| Container | `container_ID` | List of `blob_ID` values | `container_ID` | Container -> list of blobs |
| Blob | `blob_ID` | {list of chunks, chunkInfo: data node ID's,.. } | `blob_ID` | Blob -> list of chunks |
### Blob Metadata
- Blobs need an ID to find them
- If blobs are chunked, chunk IDs are required to be stored as well so a user can get all the pieces of the blob
- Chunks can be replicated to account for the loss of any of the existing chunks
### Partitioning 
REFER TO Design Considerations of a Blob Store TO COMPLETE SECTION