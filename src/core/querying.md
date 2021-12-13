{{#title Querying Atomic Data}}
# Querying Atomic Data

There are multiple ways of getting Atomic Data into some system:

- [**Atomic Collections**](../schema/collections.md) can filter, sort and paginate resources
- [**Atomic Paths**](paths.md) is a simple way to traverse Atomic Graphs and target specific values
- [**Subject Fetching**](#subject-fetching-http) requests a single subject right from its source
- [**Triple Pattern Fragments**](#triple-pattern-fragments) allows querying for specific (combinations of) Subject, Property and Value.
- [**SPARQL**](#SPARQL) is a powerful Query language for traversing linked data graphs

## Atomic Paths

An Atomic Path is a string that consist of one or more URLs, which when traversed point to an item.
For more information, see [Atomic Paths](paths.md).

## Subject fetching (HTTP)

The simplest way of getting Atomic Data when the Subject is an HTTP URL, is by sending a GET request to the subject URL.
Set the `Content-Type` header to an Atomic Data compatible mime type, such as `application/ad+json`.

```HTTP
GET https://atomicdata.dev/test HTTP/1.1
Content-Type: application/ad+json
```

The server SHOULD respond with all the Atoms of which the requested URL is the subject:

```HTTP
HTTP/1.1 200 OK
Content-Type: application/ad+json
Connection: Closed

{
  "@id": "https://atomicdata.dev/test",
  "https://atomicdata.dev/properties/shortname": "1611489928"
}
```

The server MAY also include other resources, if they are deemed relevant.

## Triple Pattern Fragments

[Triple Pattern Fragments](https://linkeddatafragments.org/specification/triple-pattern-fragments/) (TPF) is an interface for querying RDF.
It works great for Atomic Data as well.

An HTTP implementation of a TPF endpoint might accept a GET request to a URL such as this:

`http://example.org/tpf?subject={subject}&property={property}&value={value}`

Make sure to URL encode the `subject`, `property`, `value` strings.

For example, let's search for all Atoms where the value is `test`.

```HTTP
GET https://atomicdata.dev/tpf?value=0 HTTP/1.1
Content-Type: text/turtle
```

This is the HTTP response:

```HTTP
HTTP/1.1 200 OK
Content-Type: text/turtle
Connection: Closed

<https://atomicdata.dev/agents> <https://atomicdata.dev/properties/collection/currentPage> "0"^^<https://atomicdata.dev/datatypes/integer> .
```

## SPARQL

[SPARQL](https://www.w3.org/TR/rdf-sparql-query/) is a powerful RDF query language.
Since all Atomic Data is also valid RDF, it should be possible to query Atomic Data using SPARQL.

- Convert / serialize Atomic Data to RDF (for example by using the `/tpf` endpoint and an `accept` header: `curl -i -H "Accept: text/turtle" "https://atomicdata.dev/tpf"`)
- Load it into a SPARQL engine (e.g. )
