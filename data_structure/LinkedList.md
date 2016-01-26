Linked List
===========

- Find items of the list
```
MATCH (root :ListRoot { name: 'myList'})-[:LINK*..]->(items:ListItem)
RETURN root, items
```

- Create a linked list
```
CREATE (root: ListRoot { name: 'myList'})-[:LINK]->(root)
```

- Add item at the end of the list.
```
MATCH (last)-[oldLink :LINK]->(root: ListRoot {name: 'myList'})
WHERE last :ListRoot OR last :ListItem
CREATE (new :ListItem {name: 'Item1'})
CREATE (last)-[:LINK]->(new)-[:LINK]->(root)
DELETE oldLink
RETURN new
```
