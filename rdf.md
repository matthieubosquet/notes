# RDF

> The Resource Description Framework (RDF) is a framework for representing information in the Web.
>
> — [RDF 1.1 Concepts and Abstract Syntax - Introduction](https://www.w3.org/TR/rdf11-concepts/#section-Introduction)

## Structure

The core structure of RDF is a set of statements or triples, also called an RDF Graph.

Each RDF triple consists of a subject, a predicate and an object:
- a subject is an IRI or a blank node
- a predicate is an IRI
- an object is an IRI, a literal or a blank node

There exists a number of concrete syntaxes, such as Turtle, to serialize RDF.

For example, an RDF graph asserting in two statements that Alice has a name and that she knows Bob could be serialised in Turtle as such:

```turtle
prefix ex: <https://example.org/>
prefix foaf: <http://xmlns.com/foaf/0.1/>

ex:alice
  foaf:name "Alice" ;
  foaf:knows ex:bob .
```

## Vocabulary

The purpose of an RDF vocabulary is to standardise the universe of discourse by minting a set of IRIs that have a permanent meaning and can be used as predicate in an RDF statement; in other words, IRIs that denote a property or relation between two entities.

The RDF, RDFS and OWL W3C vocabularies can be used to determine the logic a property entails and hence which reasoning can be used to infer statements from asserted statements. Different entailment regimes can be applied to statements depending on the amount of logic one wants to apply to an RDF Graph.

For example, the FOAF vocabulary we used to assert that Alice has a name and knows Bob uses RDF to say: "entities that know each other are persons" as such:

```turtle
prefix foaf: <http://xmlns.com/foaf/0.1/>

foaf:knows
  rdfs:domain foaf:Person ;
  rdfs:range foaf:Person .
```

The `rdfs:domain` and `rdfs:range` respectively mandate type(s) to apply to subjects and objects in statements where the a property is used. Here `foaf:knows` implies a `foaf:Person` type as such:

```turtle
prefix ex: <https://example.org/>
prefix foaf: <http://xmlns.com/foaf/0.1/>

# Asserted statements
ex:alice
  foaf:name "Alice" ;
  foaf:knows ex:bob .

# Vocabulary statements
foaf:knows
  rdfs:domain foaf:Person ;
  rdfs:range foaf:Person .

# Inferred statements
ex:alice a foaf:Person .
ex:bob a foaf:Person .
```

## IRIs, literals & blank nodes



## References

- [RDF 1.1 Concepts and Abstract Syntax](https://www.w3.org/TR/rdf11-concepts/)
- [FOAF Vocabulary Specification](http://xmlns.com/foaf/spec/)
- [RDF Schema 1.1](https://www.w3.org/TR/rdf-schema/)
- [Internationalized Resource Identifiers (RFC3987)](https://www.ietf.org/rfc/rfc3987.html)
- []()
