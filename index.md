# Querying a Knowlege Graph with SPARQL

### Imagine you wanted a list of people influenced by French writer and philosopher Voltaire. What would have been a tedious research is possible within a simple query on Wikidata with SPARQL.

Wikidata is a free and collaborative Linked Open Data (LOD) knowledge base which is human and machine readible. The project started 2012 by the Wikimedia Foundation as an attempt to centralize and to structure human knowledge available in the online encyclopedia Wikipedia in an datafied form as RDF triples. 

The vision of the semantic web was already stated by Tim Berners-Lee, one of the founders of the world wide web in 2009. See his [TED-talk](https://www.youtube.com/watch?v=OM6XIICm_qo) on "The next web".

How are Wikidata items structured?  In this diagram, you can see the structure of a Wikidata item. Each of them has a list of statements, which are triples in the form 
```SUBJECT - PREDICATE - OBJECT``` (e.g. Douglas Adams is educated at the St John’s College). In Wikidata the subject is referred to an item and the predicate is referred to as property. Each property has a value, which can be again an item, text, number, date, or GPS coordinates among others. Each value can have additional qualifiers which have additional information with other property-value pairs such as start time. This structure will be important when you start to express queries with SPARQL.

![This is an image](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Datamodel_in_Wikidata.svg/1280px-Datamodel_in_Wikidata.svg.png)
Figure 1: Wikidata datamodel (Wikicommons)

## Introduction to SPARQL

SPARQL is the acronym of _SPARQL Protocol And RDF Query Language_ and it is used to query data stored as RDF (Resource Description Framework) and it is standardized by the W3C.

Starting with the `SELECT` clause, you define the variables you want to get (variables are prefixed with a question mark). Inside the `WHERE` clause, you set restrictions which mostly take the form of the triples you have seen previously. The statement `?item wdt:P8 ?top.` collects all items which have the property publication place `(P8)`  into the variable `?top`. 

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


You can try this query [here](https://tinyurl.com/y9w4wzkr).

Let's get back to our initial question: Who was influenced by writer Voltaire? We can use the [Wikidata Graph Builder](https://angryloki.github.io/wikidata-graph-builder) to answer that question, Voltaire being our `root item`, `influenced by` being our transversal property in mode `reverse` and using 2 iterations. The query can be exported to "Wikidata Query Service" as well: 

```
PREFIX gas: <http://www.bigdata.com/rdf/gas#>

SELECT ?item ?itemLabel ?linkTo {
  SERVICE gas:service {
    gas:program gas:gasClass "com.bigdata.rdf.graph.analytics.SSSP" ;
                gas:in wd:Q9068 ;
                gas:traversalDirection "Reverse" ;
                gas:out ?item ;
                gas:out1 ?depth ;
                gas:maxIterations 2 ;
                gas:linkType wdt:P737 .
  }
  OPTIONAL { ?item wdt:P737 ?linkTo }
  SERVICE wikibase:label {bd:serviceParam wikibase:language "en" }
}

```

You can try this query [here](https://angryloki.github.io/wikidata-graph-builder/?property=P737&item=Q9068&iterations=2&mode=reverse).

But, wait - Russian propagandist Aleksandr Dugin is influenced by Voltaire?? What is the origin of this statement? In Reasoning with Knowledge Graphs, there is a thing called inferences. 

If A is influenced by B and B is influenced by C, then we can infer that A is influenced by C, which can lead to quite absurd statements if one increases the iterations. 

So, we rather decrease the iterations back to 1 iteration. 

```
PREFIX gas: <http://www.bigdata.com/rdf/gas#>

SELECT ?item ?itemLabel ?linkTo {
  SERVICE gas:service {
    gas:program gas:gasClass "com.bigdata.rdf.graph.analytics.SSSP" ;
                gas:in wd:Q9068 ;
                gas:traversalDirection "Reverse" ;
                gas:out ?item ;
                gas:out1 ?depth ;
                gas:maxIterations 1 ;
                gas:linkType wdt:P737 .
  }
  OPTIONAL { ?item wdt:P737 ?linkTo }
  SERVICE wikibase:label {bd:serviceParam wikibase:language "en" }
}
```
You can try this query [here](https://angryloki.github.io/wikidata-graph-builder/?property=P737&item=Q9068&iterations=1&mode=reverse).

So we keep in mind, that the inferences should be succept to critical inquiry. 

Read on: 

- [Wikidata:Introduction](https://www.wikidata.org/wiki/Wikidata:Introduction)
- [Wikidata:SPARQL queries](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries)
- [Wikidata:SPARQL queries examples](https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples)
- [Wikidata:Creating a bot](https://www.wikidata.org/wiki/Wikidata:Creating_a_bot)

