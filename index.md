# What is SPARQL?

SPARQL is a query language like SQL but it queries RDF (Resource Description Framework) databases, databases that are a network of objects connected by their relations to each other. For instance, the object Library would be connected to the object Books by a "contains" relation.

# SPARQL QUERY STRUCTURE

RDF models are tripes in a subject -> predicate -> object form. The subject, predicate, and object are generally represented as URIs however they can be shortened by using PREFIX as described below.

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

Adding the DISTINCT modifier eliminates duplicate rows from the results.

```
SELECT DISTINCT ?title
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

### LIMIT

You can set the maximum results returned by a query. For example you want to limit the number for results to 3:

```
WHERE { ... } LIMIT 3
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

### UNION

Say you wanted to find all the people named "George Washington" and "John" in one search and combine the result. Then you could use UNION:

```
SELECT ?person
WHERE {
	{?person foaf:name "George Washington"@en } UNION {?person foaf:name "John"@en}
}
```

You should get a result like this:

![Washington UNION John Result](screenshots/GeorgeJohn.png?raw=true)

### FILTER

You can also filter you queries by using FILTER. SPARQL has the following built-in functions:

* **Logical**: ! (NOT), && (AND), || (OR)
* **Math**: +, -, \*, /
* **Comparison**: =, != , >, <, >=,  <=

#### Conditionals

The following example gives first 3 populated places alphabettically that have a population greater than 1,000,000,000.

```
SELECT * WHERE {
    ?place a dbo:PopulatedPlace .
    ?place foaf:name ?name .
    ?place dbo:populationTotal ?pop .
    FILTER(?pop > 1000000000) .
} ORDER BY ?name LIMIT 3
```

You should get a result like this:

![Populations Result](screenshots/Population.png?raw=true)

#### Explanation
There are many things happening in this query. First, we define that ?country should be limited to only Populated Places. Then, we get all the names of the places in name. After that, we are getting the total populations of all the places in pop. We then apply a filter that only keeps those with a population greater than 1,000,000,000. Finally, once we have our data we sort it by name and then limit our output to the first three results.

The keyword **a** is a shortcut for the predicate rdf:type.

#### Multiple Conditionals

We can build upon the previous example and apply multiple filters using a combination of the built-in SPARQL functions.

```
SELECT * WHERE {
    ?place a dbo:PopulatedPlace .
    ?place foaf:name ?name .
    ?place dbo:populationTotal ?pop .
    FILTER(?pop > 1000000000 &&
           ?pop < 2000000000 ) .
} ORDER BY ?name LIMIT 3
```

You should get a result like this:

![Populations Result with multiple conditions](screenshots/population_multiple_conditions.png?raw=true)

#### Explanation

It's the same query we've seen before with the added restriction that the population is limited between 1,000,000,000 and 2,000,000,000.

#### Regex

You can also filter your queries by using regex.

The syntax is :
```
FILTER regex(?x "pattern" [,"flags"])
```

Here the pattern is text you want to search for in ?x. The flags argument is optional. When set to "i", it performs a case-insensitive match.


```
SELECT *
WHERE {
  ?person foaf:name ?name
  FILTER regex(?name, "May")
} LIMIT 5
```

You should get a result like this:

![Populations Result](screenshots/filter_regex.png?raw=true)

#### Explanation

In this query we're searching for person who's name contains the string 'May' in it.

### OPTIONAL

SPARQL also gives users the option to only include column data for a query when a row might not have a property, like how 'LEFT' join works in SQL. For example:

```
SELECT * WHERE {

    ?person foaf:name "John Smith"@en .
    OPTIONAL { ?person dbpedia2:totalgoals ?totalgoals }

}  LIMIT 5
```

You should get a result like this:

![Total goals result](screenshots/optional_totalgoals.png?raw=true)

#### Explanation

In the query above, we're searching for a person object named John Smith with an optional relation of 'totalgoals'. Some of the results from this search do not need have 'totalgoals' because it is an optional column based on the search. As we can see from the image above, only the first 3 records have the 'totalgoals' column populated. When you have data that might not have all the same properties other than the main one you are looking for OPTIONAL makes it easy to list other properties easily.
