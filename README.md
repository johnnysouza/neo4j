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
