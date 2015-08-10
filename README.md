GDC Data Dictionary
===================

Draft proposal for modeling GDC Data Dictionary, work in progress.

One important aspect worth mentioning is that it is purposely chosen to model the dictionary using Directed Acyclic Graph (DAG). The idea behinds it is simpilicity! From a practical point of view, the data dictionary is not meant to model every aspects of the real world GDC entities and their relations with fine grain semantics. Rather, it's important to be able to express and enforce data integrity rules. So far, DAG seems to be a good fit although we should look harder to see whether there are any show-stoppers, are there any relations/rules can not be expressed using DAG and there is no way around it?

Choosing simplier design will always bring benefits in software development at all levels and phases. In our case, comparing to a general graph, DAG will be much easier to work with. Querying a DAG will be simply searching up to find parents or searching down to find children; finding siblings will require one step up and one step down, which should be performant. As any other graph search, querying a DAG with very wide or deep structure can be expensive. There should be tricks we can play to make the queries we care about performant. We can also keep it in mind while designing the model to avoid very wide or deep DAG whenever possible.

Taking one step further, practically, type of relations between nodes may not be so important to us. Just like ER modeling in RDBMS, uniqueness/cadinality is the only thing matters. With this thinking, it's possible to entirely eliminate the need for an edge table while implement the DAG model. We will still need to support some not so straightforward relations such as conditional relations, such as when there is A, there should (or shouldn't) be B etc. However such business logic is not necessarily harder for DAG to handle.

One last point is that converting data in a DAG to JSON should be easier comparing to a general graph. Data in a JSON document is essentially a tree. When converting a DAG to a tree, it is mainly to denormalize child nodes with multiple parents into multiple copies, each parent will have a materialized local child node copy. This should make the logic cleaner when we export data in graph db to JSON to build Elasticsearch indexes.

## Contributing

Read how to contribute [here](https://github.com/NCI-GDC/gdcapi/blob/master/CONTRIBUTING.md)
