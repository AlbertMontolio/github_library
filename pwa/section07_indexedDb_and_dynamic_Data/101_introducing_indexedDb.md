101_introducing_indexedDb.md

is a transactional key-value database in the browser

transaction means: if one action within the transaction fails, none of the actions of that transaction are applied

if you are writing 10 queries, and one fails, nothing is stored

you can store significant amounts of unstructured data, including files / blobs

it can be accessed asynchronously

you can use from your js code and from your sw

usually you have one database per application

in a given db, you have multiple object store, this is a table

inside object store, you store the object





