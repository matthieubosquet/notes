---
title: WebID
---

> A WebID is an HTTP URI which refers to an Agent (Person, Organization, Group, Device, etc.).
>
> ― [WebID 1.0: Introduction](https://www.w3.org/2005/Incubator/webid/spec/identity/#introduction)

## Requirements

The only requirements for a WebID Profile Document are the following:

1. It must be available as `text/turtle`;
    >  A WebID Profile Document is a Web resource that MUST be available as text/turtle...
    >
    > — [WebID 1.0: Introduction](https://www.w3.org/2005/Incubator/webid/spec/identity/#introduction)
2. It must be dereferenceable from the WebID conforming to one of the [Cool URIs solution for identifying real world objects](https://www.w3.org/TR/cooluris/#solutions);
    > For WebIDs with fragment identifiers (e.g. #me), the URI without the fragment denotes the Profile Document. For WebIDs without fragment identifiers an HTTP request on the WebID MUST return a 303 with a Location header URI referring to the Profile Document.
    >
    > — [WebID 1.0: Terminology](https://www.w3.org/2005/Incubator/webid/spec/identity/#terminology)
3. The WebID referent must control the Profile Document.
    > The WebID HTTP URI must be one that dereferences to a document the user controls.
    >
    > — [WebID 1.0: The WebID HTTP URI](https://www.w3.org/2005/Incubator/webid/spec/identity/#the-webid-http-uri)

## Public and private profiles

A WebID can link to any number of profile documents via the predicate `foaf:isPrimaryTopicOf`.

Profile documents can present any amount of information about the agent denoted by/controlling the WebID.

Profile documents can have their own permissions and hence accommodate any adequate level of privacy.

Profile documents linked from the WebID Profile Document will be discoverable.

For example, Alice could have a WebID dereferenced as such:

```http
GET /alice
HOST: example.org

HTTP/2 303
Location: /alice?webIDProfile

GET /alice?webIDProfile
HOST: example.org

HTTP/2 200
Content-Type: text/turtle

PREFIX ex: <https://example.org/>
prefix foaf: <http://xmlns.com/foaf/0.1/>

ex:alice
  foaf:isPrimaryTopicOf ex:publicProfileDocument, ex:privateProfileDocument ;
.
```

## References

- [WebID 1.0 - Web Identity and Discovery](https://www.w3.org/2005/Incubator/webid/spec/identity/)
- [Cool URIs for the Semantic Web](https://www.w3.org/TR/cooluris/#solutions)
