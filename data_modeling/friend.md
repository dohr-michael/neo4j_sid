Friend of my friend
===================

Data Initialization
-------------------

```
CREATE (luke :FriendPerson {name: 'Luke'})
CREATE (joda :FriendPerson {name: 'Joda'})
CREATE (obi :FriendPerson {name: 'Obi'})
CREATE (qui :FriendPerson {name: 'Qui-Gon'})
CREATE (windu :FriendPerson {name: 'Windu'})
CREATE (chuwi :FriendPerson {name: 'Chuwi'})
CREATE (luke)-[:knows]->(joda)
CREATE (luke)-[:knows]->(obi)
CREATE (joda)-[:knows]->(obi)
CREATE (joda)-[:knows]->(qui)
CREATE (joda)-[:knows]->(windu)
CREATE (obi)-[:knows]->(qui)
CREATE (obi)-[:knows]->(chuwi)
```

Find unknows friend of my friend
--------------------------------
```
MATCH (luke :FriendPerson {name: 'Luke'})-[:knows*2..2]-(friend_of_friend :FriednPerson)
WHERE NOT (luke)-[:knows]-(friend_of_friend)
RETURN friend_of_friend.name, count(*)
ORDER BY COUNT(*) DESC , friend_of_friend.name
```
