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

To start off, we'll query all people in the database that have names:

```
SELECT * 
WHERE {
    ?person foaf:name ?name .
}
```

You should get a result like this: 

![Query Result](screenshots/name_result.png?raw=true)

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
