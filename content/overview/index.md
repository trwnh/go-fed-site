---
title: Go-Fed Overview
summary: Go-Fed is a suite of libraries that lowers the barrier to writing Fediverse apps in Go. Libraries built under the Go-Fed umbrella follow the philosophy of both a tech "stack" with composability of other standards. For example, developers can choose to use a minimal amount of the ActivityPub stack and still opt to compose it with go/fed's library for HTTP Signatures, or to use the ActivityPub stack without using go/fed's HTTP Signatures library.
---

## The Go-Fed Project {#Go-Fed-Project}

Go-Fed is a suite of libraries that lowers the barrier to writing Fediverse apps in Go. Libraries built under the Go-Fed umbrella follow the philosophy of both a tech "stack" with composability of other standards. For example, developers can choose to use a minimal amount of the ActivityPub stack and still opt to compose it with go/fed's library for HTTP Signatures, or to use the ActivityPub stack without using go/fed's HTTP Signatures library.

A visual overview:

{{< rawhtml >}}
<div class="svg-container">
<svg height="600" width="480">
  
  <marker id="arrow" viewBox="0 0 10 10" refX="5" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
    <path d="M 0 0 L 10 5 L 0 10 z"></path>
  </marker>
  
  <text x="20" y="30" class="svgtext">go-fed libraries</text>
  <text x="295" y="30" class="svgtext">concepts</text>
  <rect width="240" height="600" class="svglibrary"></rect>
  <rect x="240" width="240" height="600" class="svglibrary"></rect>
  
  
  <rect x="245" y="495" width="230" height="100" class="svglibrary svgjsonld"></rect>
  <text x="290" y="555" class="svgtext">JSON-LD</text>
  <rect x="245" y="370" width="230" height="100" class="svglibrary svgas"></rect>
  <text x="255" y="430" class="svgtext">ActivityStreams</text>
  <line x1="360" y1="495" x2="360" y2="475" class="svgdepline" marker-end="url(#arrow)"></line>
  <rect x="245" y="245" width="230" height="100" class="svglibrary svgap"></rect>
  <text x="285" y="305" class="svgtext">ActivityPub</text>
  <line x1="360" y1="370" x2="360" y2="350" class="svgdepline" marker-end="url(#arrow)"></line>
  <rect x="360" y="120" width="115" height="100" class="svglibrary svgother"></rect>
  <text x="395" y="165" class="svgtextsmall">Other</text>
  <text x="370" y="190" class="svgtextsmaller">(Http Sigs, etc.)</text>
  <rect x="245" y="45" width="230" height="50" class="svglibrary svgapp"></rect>
  <text x="300" y="80" class="svgtext">Fedi app</text>
  <line x1="300" y1="245" x2="300" y2="100" class="svgdepline" marker-end="url(#arrow)"></line>
  <line x1="420" y1="120" x2="420" y2="100" class="svgdepline" marker-end="url(#arrow)"></line>
  
  
  <rect x="5" y="495" width="230" height="100" class="svglibrary svgjsonld"></rect>
  <text x="30" y="545" class="svgtextsmall">go-fed/activity/astool</text>
  <text x="70" y="565" class="svgtextsmaller">(at compile time)</text>
  <rect x="5" y="370" width="230" height="100" class="svglibrary svgas"></rect>
  <text x="20" y="427" class="svgtextsmall">go-fed/activity/streams</text>
  <line x1="120" y1="495" x2="120" y2="475" class="svgdepline" marker-end="url(#arrow)"></line>
  <rect x="5" y="245" width="230" height="100" class="svglibrary svgap"></rect>
  <text x="40" y="302" class="svgtextsmall">go-fed/activity/pub</text>
  <line x1="120" y1="370" x2="120" y2="350" class="svgdepline" marker-end="url(#arrow)"></line>
  <rect x="120" y="120" width="115" height="100" class="svglibrary svgother"></rect>
  <text x="135" y="175" class="svgtextsmaller">go-fed/httpsig</text>
  <rect x="5" y="45" width="230" height="50" class="svglibrary svgapp svgwip"></rect>
  <text x="60" y="75" class="svgtextsmall">go-fed/apcore</text>
  <line x1="60" y1="245" x2="60" y2="100" class="svgdepline" marker-end="url(#arrow)"></line>
  <line x1="180" y1="120" x2="180" y2="100" class="svgdepline" marker-end="url(#arrow)"></line>
</svg>
</div>
{{< /rawhtml >}}

## Libraries

The ActivityPub stack is contained in the respository located at [github.com/go-fed/activity](https://github.com/go-fed/activity). This is a monolithic repository containing three more libraries, in order to address different problems with generally mapping the ActivityPub spec into Go.

ActivityPub is built on the exchange of JSON-LD data, which is not trivial to map into Go. The `astool` command line tool reads RDF schema definitions in order to generate code that produces strong types that readily serialize and deserialize to and from JSON-LD. The JSON-LD understanding is done at code-generation time, so your federating application can generally skip using this tool. you are interested in bringing new RDF vocabularies into go-fed, this is the tool that makes it happen.

The ActivityStreams vocabulary is the basic vocabulary of ActivityPub. It is an RDF schema, shared between peers as JSON-LD, and drives core behaviors between Federated applications. The `streams` library is generated by `astool` to let federating apps in Go easily and safely manipulate ActivityStreams types.

Finally, the `pub` library implements the barest bones behaviors required by the ActivityPub specification. It is designed around the actor model, and relies on Go apps to implement quite a few interfaces. The default behaviors help get a new app off the ground, but veteran applications built on Go-Fed will find these behaviors customizable or entirely overridable.

In the future, the [github.com/go-fed/apcore](https://github.com/go-fed/apcore) library will be an opinionated, heavyweight framework to really speed up the minimal code required to get a Federated app going. This is built on top of the [github.com/go-fed/activity](https://github.com/go-fed/activity) library, and offers some of the same flexibility, but also injects more of the community's de-facto standards instead of narrowly sticking to the ActivityPub specification.

An implementation of HTTP Signatures is provided by the [github.com/go-fed/httpsig](https://github.com/go-fed/httpsig) library. While the HTTP Signatures specification is not mentioned in the ActivityPub specification, it has been adopted by the community. It can be used independently of the other libraries in the Go-Fed suite.

## Next Steps

Excited? The next step depends what you are looking for!

- Want to learn how to manipulate ActivityStreams data? Head on over to the [activity/streams reference]({{< ref "/ref/activity/streams" >}})
- Want to learn how ActivityPub has been isolated into a library form? The [activity/pub reference]({{< ref "/ref/activity/pub" >}}) has the details.
- Want to use Go-Fed to get a small federated app off the ground? Head over to the [App Tutorial]({{< ref "/activitypub-tutorial" >}})
- Want to learn more about ActivityPub in general? Try [ActivityPub Protocol At A Glance]({{< ref "/activitypub-glance" >}})