# Querying a Knowlege Graph with SPARQL

Imagine you wanted a list of people influenced by French writer and philosopher Voltaire. What would have been a tedious research is possible within a simple query on Wikidata with SPARQL.

Wikidata is a free and collaborative Linked Open Data (LOD) knowledge base which is human and machine readible. The project started 2012 by the Wikimedia Foundation as an attempt to centralize and to structure human knowledge available in the online encyclopedia Wikipedia in an datafied form as RDF triples. 

The vision of the semantic web was already stated by Tim Berners-Lee, one of the founders of the world wide web in 2009. See his [TED-talk](https://www.youtube.com/watch?v=OM6XIICm_qo) on "The next web".

How are Wikidata items structured?  In this diagram, you can see the structure of a Wikidata item. Each of them has a list of statements, which are triples in the form 
```SUBJECT - PREDICATE - OBJECT``` (e.g. Douglas Adams is educated at the St John’s College). In Wikidata the subject is referred to an item and the predicate is referred to as property. Each property has a value, which can be again an item, text, number, date, or GPS coordinates among others. Each value can have additional qualifiers which have additional information with other property-value pairs such as start time. This structure will be important when you start to express queries with SPARQL.

![This is an image](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Datamodel_in_Wikidata.svg/1280px-Datamodel_in_Wikidata.svg.png)
Figure 1: Wikidata datamodel (Wikicommons)

## Introduction to SPARQL

SPARQL is the acronym of _SPARQL Protocol And RDF Query Language_ and it is used to query data stored as RDF (Resource Description Framework) and it is standardized by the W3C.

Starting with the `SELECT` clause, you define the variables you want to get (variables are prefixed with a question mark). Inside the ` WHERE` clause, you set restrictions which mostly take the form of the triples you have seen previously. The statement `?item wdt:P8 ?top.` collects all items which have the property publication place `(P8)`  into the variable `?top`. 

```
SELECT ?topLabel (count(*) as ?count)
WHERE {
?item wdt:P8 ?top. #P8 = ‘publication location’
?top rdfs:label ?topLabel .
filter(lang(?topLabel) = "en")
}
GROUP BY ?topLabel
ORDER BY desc(?count)
``` 

The SPARQL endpoint in Wikibase (open source software behind Wikidata) comes along with several visualisations. The above query can be visualised for example in a BubbleChart: 

![This is an image](https://www.mimotext.uni-trier.de/application/files/7216/4820/6455/query1_overview_publication_places.png)
Figure 2: Overview of the places of publication.


You can try this query [here](http://zora.uni-trier.de:11100/#).
