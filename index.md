# What is SPARQL?

SPARQL is a query language like SQL but it queries RDF (Resource Description Framework) databases, databases that are a network of objects connected by their relations to each other. For instance, the object Library would be connected to the object Books by a "contains" relation. 

# SPARQL QUERY STRUCTURE 

### PREFIX

Namespace for possible relations to query (i.e. the prefix for querying book objects would have a title relation)

```
PREFIX book-info: <http://www.example.com/books-info>
````

### SELECT

Determines what set of queried data is returned (i.e. the titles of selected books)

```
SELECT ?title
```

*** Note: In SPARQL, variable names start with '?' ***

### FROM

Determines the database to be queried (i.e. a library's inventory site)

```
FROM <http://www.example.com/books-info.rdf>
```

### WHERE

Determines the pattern that you are looking for within the dataset (i.e. return all books that have titles)

```
WHERE { ?books book-info:title ?title }
```

*** Note: WHERE clauses are made up of the triple: subject, predicate, and object ***

### ORDER BY (OTHER RESULT MODIFIERS)

Organize the query results in a specific way (i.e. by the titles of the returned books)

```
ORDER BY ?title
```

# MAKING YOUR OWN QUERY

In order to make a SPARQL query, you need a SPARQL endpoint (a site that will run your query on a RDF database). For this tutorial we will be using a dbpedia SPARQL query endpoint: [dbpedia.org/snorql/](http://dbpedia.org/snorql/)

If you go to the endpoint website you'll see a few different prefixs that the endpoint already has setup (owl, xsd, rdfs, etc...). For our first query we'll use the "foaf" prefix, which is a prefix for decscribing people, their relationships, activities, etc...

### QUERYING ALL NAMES 

To start off, we'll query all people in the database that have names:

```
SELECT * 
WHERE {
    ?person foaf:name ?name .
}
```

You should get a result like this: 

![Name Query Result](screenshots/name_result.PNG?raw=true)

#### Explanation

For this query we used the WHERE clause "?person foaf:name ?name .", this clause looks for all person classes that have a name class. Then in the SELECT statement we used the wildcard operator from SQL (\*) to return all the variables (?person and ?name) we set in the WHERE clause. Thus our result has two columns, person (which has the person class that links to all the info about that person), and name (which is the string that is the name for that person)

*** Note: We did not need to use FROM operator in our SPARQL statement because running the query on this endpoint automatically runs the query on the dbpedia database ***

### QUERYING A SPECIFIC NAME

For the next query, we'll get a little more specific. We'll query all people classes that have the name "George Washington" and return the person classes:

```
SELECT ?person 
WHERE {
    ?person foaf:name "George Washington"@en .
}
```

You should get a result like this:

![Washington Query Result](screenshots/washington_result.PNG?raw=true)

#### Explanation

For this query we used the same WHERE clause as in the first query but we switched the variable ?name with an actual name string ("George Washington"). So instead of looking for all person classes with a name, it looked for all person classess that contained "George Washington" in the name. Then in our SELECT statement we specified we just wanted the ?person variable, thus our result was just one column with all the person classes that matched the WHERE clause.  
