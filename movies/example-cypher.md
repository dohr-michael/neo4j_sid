Examples :
==========



> Heroes of Might and Magic

> Batman Forever


Movie with `Batman` :
> MATCH (m: Movie) WHERE UPPER(m.name) =~ '.*BATMAN.*' RETURN m.name

Limit to 100 movies :
> MATCH (m: Movie) RETURN m.name LIMIT 100

Sum of votes :
> MATCH (m: Movie) RETURN sum(m.votes)

Average of ratings  (if ratings > 0):
> MATCH (m: Movie) WHERE m.rating > 0 RETURN sum(m.rating) / count(m)


Kevin Spacey Best note
> MATCH (m: Movie)<-[:ACT_IN]-(k: Actor {firstName: 'Kevin', lastName: 'Spacey'}) RETURN m.name, m.rating ORDER BY m.rating DESC LIMIT 1

Movies per actor

```
MATCH (:Movie)<-[r:ACT_IN]-(a: Actor)
WITH a, count(r) as nbm
RETURN a.firstName, a.lastName, nbm ORDER BY nbm DESC
```

Movies category for the winner

```
MATCH (:Actor {firstName: 'Alex', lastName: 'Sanders'})-[:ACT_IN]->(:Movie)-[:IS_A]->(cat:Genre)
RETURN DISTINCT cat, count(cat) ORDER BY count(cat) DESC
```

Movies and Roles of Kevin Spacey
> MATCH (m: Movie)<-[r:ACT_IN]-(:Actor {firstName: 'Kevin', lastName: 'Spacey'}) RETURN m.name, r.role

All actors of Batman Forever
> MATCH (:Movie {name: 'Batman Forever'})<-[r:ACT_IN]-(a:Actor) RETURN a.firstName, a.lastName

Co-actors of Kevin Spacey
> MATCH (k:Actor {firstName: 'Kevin', lastName: 'Spacey'})-[:ACT_IN]->(m:Movie)<-[:ACT_IN]-(coActor:Actor) RETURN coActor.firstName, coActor.lastName, count(m)

Movies of Kevin Bacon

Co-actors haven't no works with Kevin Bacon
```
MATCH (kev:Actor {firstName: 'Kevin', lastName: 'Bacon'})-[:ACT_IN]->(m)<-[:ACT_IN]-(coActors),
      (coActors)-[:ACT_IN]->(m2)<-[:ACT_IN]-(cocoActors)
WHERE NOT (tom)-[:ACT_IN]->(m2)
RETURN cocoActors.name AS Recommended, count(*) AS Strength ORDER BY Strength DESC
```

```
MATCH (kev:Actor {firstName: 'Kevin', lastName: 'Bacon'})-[:ACT_IN]->(m)<-[:ACT_IN]-(coActors),
      (coActors)-[:ACT_IN]->(m2)<-[:ACT_IN]-(dm:Actor {firstName: "Dennis", lastName: "Miller"})
RETURN kev, m, coActors, m2, dm
```

Short path between Kevin Bacon and Kevin Spacey

```
MATCH (bacon:Actor {firstName: 'Kevin', lastName: 'Bacon'}), (spacey:Actor {firstName: 'Kevin', lastName: 'Spacey'}),
    p = shortestPath((bacon)-[*]-(spacey))
WHERE ALL (r IN rels(p) WHERE type(r) = 'ACT_IN')
RETURN p
```
