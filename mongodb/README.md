Installation guide :
====================

http://neo4j.com/developer/neo4j-doc-manager/



Run :
-----

> mongod --replSet myDevReplSet

> mongo-connector -c ./config.json

> db neo, collections users

> MATCH (n:Document) RETURN n

> db.users.insert({name: 'Michael', groups: [{groups_id: ObjectId('56a86aff70958599966712e3')}, {groups_id: ObjectId('56a86b0b70958599966712e5')}]})
