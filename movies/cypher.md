Import Data
===========

Movies
------

```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/movies/movies.csv' AS line FIELDTERMINATOR '#'
// Format : Name,Rating,Votes,Type,ReleaseYear,Number,Suspended
CREATE (:Movie {name: line.Name, rating: toFloat(line.Rating), votes: toInt(line.Votes), type: line.Type, releaseYear: toInt(line.ReleaseYear), suspended: (line.Suspended = 'true')})
```

Actors
------

```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/movies/actors.csv' AS line FIELDTERMINATOR '#'
// Format : Name,LastName
CREATE (:Actor {firstName: line.Name, lastName: line.LastName})
```

Countries
---------

```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/movies/countries.csv' AS line FIELDTERMINATOR '#'
// Format : Name
CREATE (:Country {name: line.Name})
```

Genres
------

```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/movies/genres_cat.csv' AS line FIELDTERMINATOR '#'
// Format : Name
CREATE (g:Genre {name: line.Name})
```


Indexes
-------
```
CREATE INDEX ON :Movie(name)
```
```
CREATE INDEX ON :Actor(firstName)
```
```
CREATE INDEX ON :Actor(lastName)
```

Links
-----

```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/movies/actors_movies.csv' AS line FIELDTERMINATOR '#'
// Format : Movie,Actor,Role
MATCH (m:Movie),(a:Actor)
WHERE m.name = line.Movie
AND line.Actor = a.firstName + '-' + a.lastName
CREATE (a)-[:ACT_IN {role: line.Role}]->(m)
```

```
LOAD CSV WITH HEADERS FROM 'https://raw.githubusercontent.com/dohr-michael/neo4j_sid/master/movies/genres.csv' AS line FIELDTERMINATOR '#'
// Format : Name,Genre
MATCH (m:Movie {name: line.Name}),(a:Genre {name: line.Genre})
CREATE (m)-[:IS_A]->(a)
```
