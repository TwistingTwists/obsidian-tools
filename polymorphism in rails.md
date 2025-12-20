

- delegated types in rails (recording, recordables)
- recordings -- mutable, recordable -> immutable
- recordings are organised in a tree - parent-child
- events -> reference to a recording -> references recordable
- 

document (recordable) can have comments (recordable)

### query patterns:
#### filter on type of recordable
- recording.recordable
- recordings.messages -> recordings that have a type of message 
- recordings.messages.comments -> recordings that have a type of message -> comments


Bucket is a delegated type -> the actual (delegated) type is Bucketable
Recording is delegated type -> the actual type is Recordable

Bucket = concept - container of recordings



recordings have children
- > recordable (what kind of - message / comment etc)

bucket - have access (who can access those recordings)
- recordings lie in bucket

anything that can contain recordings => bucket