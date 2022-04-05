## Querying Wikidata with SPARQL

Imagine you wanted a list of people influenced by French writer and philosopher Voltaire. What would have been a tedious research is possible within a simple query on Wikidata with SPARQL.

Wikidata is a free and collaborative Linked Open Data (LOD) knowledge base which is human and machine readible. The project started 2012 by the Wikimedia Foundation as an attempt to centralize and to structure human knowledge available in the online encyclopedia Wikipedia in an datafied form as RDF triples. 

The vision of the semantic web was already stated by Tim Berners-Lee, one of the founders of the world wide web in 2009. See his [TED-talk](https://www.youtube.com/watch?v=OM6XIICm_qo)on "The next web".

How are Wikidata items structured?  In this diagram, you can see the structure of a Wikidata item. Each of them has a list of statements, which are triples in the form 
```SUBJECT - PREDICATE - OBJECT``` (e.g. Douglas Adams is educated at the St John’s College). In Wikidata the subject is referred to an item and the predicate is referred to as property. Each property has a value, which can be again an item, text, number, date, or GPS coordinates among others. Each value can have additional qualifiers which have additional information with other property-value pairs such as start time. This structure will be important when you start to express queries with SPARQL.

![This is an image](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Datamodel_in_Wikidata.svg/1280px-Datamodel_in_Wikidata.svg.png)
Wikidata datamodel (Wikicommons)

You can use the [editor on GitHub](https://github.com/roettger/github.io.-/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](https://www.youtube.com/watch?v=OM6XIICm_qo) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/roettger/github.io.-/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
