Import Data :
=============
- Import Users
```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/user_management/users.csv' AS line
CREATE (:User {ref: toInt(line.Id), name: line.Name, email: line.Email})
```

- Import Groups
```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/user_management/groups.csv' AS line
CREATE (:Group {ref: toInt(line.Id), name: line.Name})
```

- Import Rights
```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/user_management/rights.csv' AS line
CREATE (:Right {ref: toInt(line.Id), name: line.Name})
```

- Create Links Between Users and Groups
```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/user_management/users_groups.csv' AS line
MATCH (u: User {ref: toInt(line.User)}), (g: Group {ref: toInt(line.Group)})
CREATE u-[:BELONG_TO]->g
```

- Create Links Between Groups
```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/user_management/groups_groups.csv' AS line
MATCH (g1: Group {ref: toInt(line.Group)}), (g2: Group {ref: toInt(line.PartOf)})
CREATE g1-[:PART_OF]->g2
```

- Create Links Between Groups and Rights
```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/user_management/groups_rights.csv' AS line
MATCH (g: Group {ref: toInt(line.Group)}), (r: Right {ref: toInt(line.Right)})
CREATE g-[:HAS]->r
```
