Import Data
===========

Movies
------

```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/movies/actors.csv' AS line FIELDTERMINATOR '#'
// Format : Name,Rating,Votes,Type,ReleaseYear,Number,Suspended
CREATE (:Movie {name: line.Name, rating: toFloat(line.Rating), votes: toInt(line.Votes), type: line.Type, releaseYear: toInt(line.ReleaseYear), suspended: (line.Suspended = 'true')})
```

Actors
------

```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/movies/actors.csv' AS line FIELDTERMINATOR '#'
// Format : Name,LastName
CREATE (:Actor {firstName: line.Name, lastName: line.LastName})
````
