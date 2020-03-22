# neo4j
Exercícios NoSql do Neo4j

Exercise 1: Retrieving Nodes

- Exercise 1.1: Retrieve all nodes from the database.
  - <code> MATCH (n) RETURN n </code>

- Exercise 1.2: Examine the data model for the graph.
  - <code> CALL db.schema() </code>

- Exercise 1.3: Retrieve all Person nodes.
  - <code> MATCH (p:Person) RETURN p </code>

- Exercise 1.4: Retrieve all Movie nodes.
  - <code> MATCH (m:Movie) RETURN m </code>
  
  Exercise 2: Filtering queries using property values
  
- Exercise 2.1: Retrieve all movies that were released in a specific year.
  - <code> MATCH (m:Movie {released:2003}) RETURN m </code>

- Exercise 2.2: View the retrieved results as a table.
  - <code> {
  "title": "The Matrix Reloaded",
  "tagline": "Free your mind",
  "released": 2003
}
{
  "title": "The Matrix Revolutions",
  "tagline": "Everything that has a beginning has an end",
  "released": 2003
}
{
  "title": "Something's Gotta Give",
  "released": 2003
} </code>

- Exercise 2.3: Query the database for all property keys.
  - <code> CALL db.propertyKeys </code>

- Exercise 2.4: Retrieve all Movies released in a specific year, returning their titles.
  - <code> MATCH (m:Movie {released: 2006}) RETURN m.title </code>

- Exercise 2.5: Display title, released, and tagline values for every Movie node in the graph.
  - <code> MATCH (m:Movie) RETURN m.title, m.released, m.tagline </code>

- Exercise 2.6: Display more user-friendly headers in the table.
  - <code> MATCH (m:Movie) RETURN m.title AS `movie title`, m.released AS released, m.tagline AS tagLine </code>
  
  Exercise 3: Filtering queries using relationships

- Exercise 3.1: Display the schema of the database.
  - <code> CALL db.schema </code>

- Exercise 3.2: Retrieve all people who wrote the movie Speed Racer.
  - <code> MATCH (p:Person)-[:WROTE]->(:Movie {title: 'Speed Racer'}) RETURN p.name </code>

- Exercise 3.3: Retrieve all movies that are connected to the person, Tom Hanks.
  - <code> MATCH (m:Movie)<--(:Person {name: 'Tom Hanks'}) RETURN m.title </code>

- Exercise 3.4: Retrieve information about the relationships Tom Hanks had with the set of movies retrieved earlier.
  - <code> MATCH (m:Movie)-[rel]-(:Person {name: 'Tom Hanks'}) RETURN m.title, type(rel) </code>

- Exercise 3.5: Retrieve information about the roles that Tom Hanks acted in.
  - <code> MATCH (m:Movie)-[rel:ACTED_IN]-(:Person {name: 'Tom Hanks'}) RETURN m.title, rel.roles </code>
  
  Exercise 4: Filtering queries using the WHERE clause
  
- Exercise 4.1: Retrieve all movies that Tom Cruise acted in.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE a.name = 'Tom Cruise'
RETURN m.title as Movie </code>

- Exercise 4.2: Retrieve all people that were born in the 70’s.
  - <code> MATCH (a:Person)
WHERE a.born >= 1970 AND a.born < 1980
RETURN a.name as Name, a.born as `Year Born` </code>

- Exercise 4.3: Retrieve the actors who acted in the movie The Matrix who were born after 1960.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE a.born > 1960 AND m.title = 'The Matrix'
RETURN a.name as Name, a.born as `Year Born` </code>

- Exercise 4.4: Retrieve all movies by testing the node label and a property.
  - <code> MATCH (m)
WHERE m:Movie AND m.released = 2000
RETURN m.title </code>

- Exercise 4.5: Retrieve all people that wrote movies by testing the relationship between two nodes.
  - <code> MATCH (a)-[rel]->(m)
WHERE a:Person AND type(rel) = 'WROTE' AND m:Movie
RETURN a.name as Name, m.title as Movie </code>

- Exercise 4.6: Retrieve all people in the graph that do not have a property.
  - <code> MATCH (a:Person)
WHERE NOT exists(a.born)
RETURN a.name as Name </code>

- Exercise 4.7: Retrieve all people related to movies where the relationship has a property.
  - <code> MATCH (a:Person)-[rel]->(m:Movie)
WHERE exists(rel.rating)
RETURN a.name as Name, m.title as Movie, rel.rating as Rating </code>

- Exercise 4.8: Retrieve all actors whose name begins with James.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(:Movie)
WHERE a.name STARTS WITH 'James'
RETURN a.name </code>

- Exercise 4.9: Retrieve all all REVIEW relationships from the graph with filtered results.
  - <code> MATCH (:Person)-[r:REVIEWED]->(m:Movie)
WHERE toLower(r.summary) CONTAINS 'fun'
RETURN  m.title as Movie, r.summary as Review, r.rating as Rating </code>

- Exercise 4.10: Retrieve all people who have produced a movie, but have not directed a movie.
  - <code> MATCH (a:Person)-[:PRODUCED]->(m:Movie)
WHERE NOT ((a)-[:DIRECTED]->(:Movie))
RETURN a.name, m.title </code>

- Exercise 4.11: Retrieve the movies and their actors where one of the actors also directed the movie.
  - <code> MATCH (a1:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(a2:Person)
WHERE exists( (a2)-[:DIRECTED]->(m) )
RETURN  a1.name as Actor, a2.name as `Actor/Director`, m.title as Movie </code>

- Exercise 4.12: Retrieve all movies that were released in a set of years.
  - <code> MATCH (m:Movie)
WHERE m.released in [2000, 2004, 2008]
RETURN m.title, m.released </code>

- Exercise 4.13: Retrieve the movies that have an actor’s role that is the name of the movie.
  - <code> MATCH (a:Person)-[r:ACTED_IN]->(m:Movie)
WHERE m.title in r.roles
RETURN  m.title as Movie, a.name as Actor </code>

Exercise 5: Controlling query processing

- Exercise 5.1: Retrieve data using multiple MATCH patterns.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)<-[:DIRECTED]-(d:Person),
      (a2:Person)-[:ACTED_IN]->(m)
WHERE a.name = 'Gene Hackman'
RETURN m.title as movie, d.name AS director , a2.name AS `co-actors` </code>

- Exercise 5.2: Retrieve particular nodes that have a relationship.
  - <code> MATCH (p1:Person)-[:FOLLOWS]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2 </code>

- Exercise 5.3: Modify the query to retrieve nodes that are exactly three hops away.
  - <code> MATCH (p1:Person)-[:FOLLOWS*3]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2 </code>

- Exercise 5.4: Modify the query to retrieve nodes that are one and two hops away.
  - <code> MATCH (p1:Person)-[:FOLLOWS*1..2]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2 </code>

- Exercise 5.5: Modify the query to retrieve particular nodes that are connected no matter how many hops are required.
  - <code> MATCH (p1:Person)-[:FOLLOWS*]-(p2:Person)
WHERE p1.name = 'James Thompson'
RETURN p1, p2 </code>

- Exercise 5.6: Specify optional data to be retrieved during the query.
  - <code> MATCH (p:Person)
WHERE p.name STARTS WITH 'Tom'
OPTIONAL MATCH (p)-[:DIRECTED]->(m:Movie)
RETURN p.name, m.title </code>

- Exercise 5.7: Retrieve nodes by collecting a list.
  - <code> MATCH (p:Person)-[:ACTED_IN]->(m:Movie)
RETURN p.name as actor, collect(m.title) AS `movie list` </code>

- Exercise 5.8: Retrieve all movies that Tom Cruise has acted in and the co-actors that acted in the same movie by collecting a list 
  - <code> MATCH (p:Person)-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(p2:Person)
WHERE p.name ='Tom Cruise'
RETURN m.title as movie, collect(p2.name) AS `co-actors` </code>

- Exercise 5.9: Retrieve nodes as lists and return data associated with the corresponding lists.
  - <code> MATCH (p:Person)-[:REVIEWED]->(m:Movie)
RETURN m.title as movie, count(p) as numReviews, collect(p.name) as reviewers </code>

- Exercise 5.10: Retrieve nodes and their relationships as lists.
  - <code> MATCH (d:Person)-[:DIRECTED]->(m:Movie)<-[:ACTED_IN]-(a:Person)
RETURN d.name AS director, count(a) AS `number actors` , collect(a.name) AS `actors worked with` </code>

- Exercise 5.11: Retrieve the actors who have acted in exactly five movies.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WITH  a, count(a) AS numMovies, collect(m.title) AS movies
WHERE numMovies = 5
RETURN a.name, movies </code>

- Exercise 5.12: Retrieve the movies that have at least 2 directors with other optional data.
  - <code> MATCH (m:Movie)
WITH m, size((:Person)-[:DIRECTED]->(m)) AS directors
WHERE directors >= 2
OPTIONAL MATCH (p:Person)-[:REVIEWED]->(m)
RETURN  m.title, p.name </code>

- Exercise 6: Controlling results returned

- Exercise 6.1: Execute a query that returns duplicate records.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released >= 1990 AND m.released < 2000
RETURN DISTINCT m.released, m.title, collect(a.name) </code>

- Exercise 6.2: Modify the query to eliminate duplication.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released >= 1990 AND m.released < 2000
RETURN  m.released, collect(m.title), collect(a.name) </code>

- Exercise 6.3: Modify the query to eliminate more duplication.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released >= 1990 AND m.released < 2000
RETURN  m.released, collect(DISTINCT m.title), collect(a.name) </code>

- Exercise 6.4: Sort results returned.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE m.released >= 1990 AND m.released < 2000
RETURN  m.released, collect(DISTINCT m.title), collect(a.name)
ORDER BY m.released DESC </code>

- Exercise 6.5: Retrieve the top 5 ratings and their associated movies.
  - <code> MATCH (:Person)-[r:REVIEWED]->(m:Movie)
RETURN  m.title AS movie, r.rating AS rating
ORDER BY r.rating DESC LIMIT 5 </code>

- Exercise 6.6: Retrieve all actors that have not appeared in more than 3 movies.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WITH  a,  count(a) AS numMovies, collect(m.title) AS movies
WHERE numMovies <= 3
RETURN a.name, movies </code>

Exercise 7: Working with Cypher data

- Exercise 7.1: Collect and use lists.
  - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie),
      (p:Person)-[:PRODUCED]->(m)
WITH  m, collect(DISTINCT a.name) AS cast, collect(DISTINCT p.name) AS producers
RETURN DISTINCT m.title, cast, producers
ORDER BY size(cast) </code>

- Exercise 7.2: Collect a list.
   - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WITH  a, collect(m) AS movies
WHERE size(movies) > 5
RETURN a.name, movies </code>

- Exercise 7.3: Unwind a list.
   - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WITH  a, collect(m) AS movies
WHERE size(movies) > 5
WITH a, movies UNWIND movies AS movie
RETURN a.name, movie.title </code>

- Exercise 7.4: Perform a calculation with the date type.
   - <code> MATCH (a:Person)-[:ACTED_IN]->(m:Movie)
WHERE a.name = 'Tom Hanks'
RETURN m.title AS title, m.released AS released, date().year - m.released AS `released age`, m.released - a.born AS `Tom age` </code>

Exercise 8: Creating Nodes

- Exercise 8.1: Create a Movie node.
  - <code> CREATE (:Movie {title: 'Forrest Gump'}) </code>

- Exercise 8.2: Retrieve the newly-created node.
  - <code> MATCH (m:Movie)
WHERE m.title = 'Forrest Gump'
RETURN m </code>

- Exercise 8.3: Create a Person node.
  - <code> CREATE (:Person {name: 'Robin Wright'}) </code>

- Exercise 8.4: Retrieve the newly-created node.
  - <code> MATCH (p:Person)
WHERE p.name = 'Robin Wright'
RETURN p </code>

- Exercise 8.5: Add a label to a node.
  - <code> MATCH (m:Movie)
WHERE m.released < 2010
SET m:OlderMovie </code>

- Exercise 8.6: Retrieve the node using the new label.
  - <code> MATCH (m:OlderMovie)
RETURN m.title, m.released </code>

- Exercise 8.7: Add the Female label to selected nodes.
  - <code> MATCH (p:Person)
WHERE p.name STARTS WITH 'Robin'
SET p:Femal </code>

- Exercise 8.8: Retrieve all Female nodes.
  - <code> MATCH (p:Female)
RETURN p </code>

- Exercise 8.9: Remove the Female label from the nodes that have this label.
  - <code> MATCH (p:Female)
REMOVE p:Female </code>

- Exercise 8.10: View the current schema of the graph.
  - <code> CALL db.schema </code>

- Exercise 8.11: Add properties to a movie.
  - <code> MATCH (m:Movie)
WHERE m.title = 'Forrest Gump'
SET m:OlderMovie, m.released = 1994, m.tagline = 'Life is like a box of chocolates…​you never know what you’re gonna get', m.lengthInMinutes = 142 </code>

- Exercise 8.12: Retrieve an OlderMovie node to confirm the label and properties.
  - <code> MATCH (m:OlderMovie)
WHERE m.title = 'Forrest Gump'
RETURN m </code>

- Exercise 8.13: Add properties to the person, Robin Wright.
  - <code> MATCH (p:Person)
WHERE p.name = 'Robin Wright'
SET p.born = 1966, p.birthPlace = 'Dallas' </code>

- Exercise 8.14: Retrieve an updated Person node.
  - <code> MATCH (p:Person)
WHERE p.name = 'Robin Wright'
RETURN p </code>

- Exercise 8.15: Remove a property from a Movie node.
  - <code> MATCH (m:Movie)
WHERE m.title = 'Forrest Gump'
REMOVE m.lengthInMinutes </code>

- Exercise 8.16: Retrieve the node to confirm that the property has been removed.
  <code> MATCH (m:Movie)
WHERE m.title = 'Forrest Gump'
RETURN m </code>

- Exercise 8.17: Remove a property from a Person node.
  - <code> MATCH (p:Person)
WHERE p.name = 'Robin Wright'
REMOVE p.birthPlace </code>

- Exercise 8.18: Retrieve the node to confirm that the property has been removed.
  - <code> MATCH (p:Person)
WHERE p.name = 'Robin Wright'
RETURN p </code>

Exercise 9: Creating Relationships

- Exercise 9.1: Create ACTED_IN relationships.
  - <code> MATCH (a:Person), (m:Movie)
WHERE a.name IN ['Robin Wright', 'Tom Hanks', 'Gary Sinise'] AND m.title = 'Forrest Gump'
CREATE (a)-[:ACTED_IN]->(m) </code>

- Exercise 9.2: Create DIRECTED relationships.
  - <code> MATCH (d:Person), (m:Movie)
WHERE d.name = 'Robert Zemeckis' AND m.title = 'Forrest Gump'
CREATE (d)-[:DIRECTED]->(m) </code>

- Exercise 9.3: Create a HELPED relationship.
  - <code> MATCH (p1:Person), (p2:Person)
WHERE p1.name = 'Tom Hanks' AND p2.name = 'Gary Sinise'
CREATE (p1)-[:HELPED]->(p2) </code>

- Exercise 9.4: Query nodes and new relationships.
  - <code> MATCH (p:Person)-[r]->(m:Movie)
WHERE m.title =  'Forrest Gump'
RETURN p, r, m </code>

- Exercise 9.5: Add properties to relationships.
  - <code> MATCH (a:Person)-[r:ACTED_IN]->(m:Movie)
WHERE m.title = 'Forrest Gump'
SET r.roles =
CASE a.name
  WHEN 'Tom Hanks' THEN ['Forrest Gump']
  WHEN 'Robin Wright' THEN ['Jenny Curran']
  WHEN 'Gary Sinise' THEN ['Lieutenant Dan Taylor']
END </code>

- Exercise 9.6: Add a property to the HELPED relationship.
  - <code> MATCH (p1:Person)-[r:HELPED]->(p2:Person)
WHERE p1.name = 'Tom Hanks' AND p2.name = 'Gary Sinise'
SET r.research = 'war history' </code>

- Exercise 9.7: View the current list of property keys in the graph.
  - <code> call db.propertyKeys </code>

- Exercise 9.8: View the current schema of the graph.
  - <code> call db.schema </code>

- Exercise 9.9: Retrieve the names and roles for actors.
  - <code> MATCH (a:Person)-[r:ACTED_IN]->(m:Movie)
WHERE m.title = 'Forrest Gump'
RETURN a.name, r.roles </code>

- Exercise 9.10: Retrieve information about any specific relationships.
  - <code> MATCH (p1:Person)-[r:HELPED]-(p2:Person)
RETURN p1.name, r, p2.name </code>

- Exercise 9.11: Modify a property of a relationship.
  - <code> MATCH (a:Person)-[r:ACTED_IN]->(m:Movie)
WHERE a.name = 'Gary Sinise' AND m.title = 'Forrest Gump' 
SET r.roles = ['Lt. Dan Taylor.'] </code>

- Exercise 9.12: Remove a property from a relationship.
  - <code> MATCH (p1:Person)-[r:HELPED]->(p2:Person)
WHERE p1.name = 'Tom Hanks' and p2.name = 'Gary Sinise' 
REMOVE r.research </code>

- Exercise 9.13: Confirm that your modifications were made to the graph.
  - <code> MATCH (p:Person)-[r:ACTED_IN]->(m:Movie)
WHERE m.title = 'Forrest Gump'
return p, r, m </code>
