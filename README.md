<img src=".github/rdf-array-logo.png" alt="[rdf-tests]" width="96" align="right">

# rdf-tests-array: rdf-tests for rdf-array

**rdf-array** fork of [rdf-tests](https://github.com/w3c/rdf-tests): the W3C RDF 1.1/1.2 syntax and semantics suites and the SPARQL 1.0/1.1/1.2 suites with their expectations rewritten for rdf-array: a collection `( … )` is one sequence term, `()` the empty sequence, and expected N-Triples and N-Quads say so (N-Triples-array, N-Quads-array). Every rewrite is the W3C expectation with its `rdf:first`/`rdf:rest` ladders folded, checked against an rdf-array parser reading the test input; each changed test carries an `rdfs:comment "rdf-array: …"`. New tests live in [rdf-tests-contrib](https://github.com/rdf-array/rdf-tests-contrib). Branch `rdf-sequences`, published at <https://rdf-array.github.io/rdf-tests-array/>; what changed and how it was checked is in [doc/rdf-array.md](doc/rdf-array.md). Part of the [rdf-array](https://github.com/rdf-array/rdf-array) organisation.

# rdf-tests
Repository for the [RDF Tests Community Group]

To view (HTML) files can be [read directly](https://w3c.github.io/rdf-tests/).

The index.html files published on GitHub pages are stored in the `gh-pages` branch and automatically built by the CI using a Ruby tool-chain based on the corresponding manifest.ttl files. Update using `rake`. Requires Ruby, rdf-turtle and json-ld gems to be installed.

Currently, the test suites for RDF ([rdf/](rdf/)) and SPARQL ([sparql/](sparql/)) are jointly managed by the [RDF Tests Community Group] and the [RDF & SPARQL Working Group].

[RDF Tests Community Group]: https://www.w3.org/groups/cg/rdf-tests/
[RDF & SPARQL Working Group]: https://www.w3.org/groups/wg/rdf-star/
