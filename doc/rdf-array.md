# rdf-tests-array: the W3C RDF and SPARQL test suites for rdf-array

This fork of [w3c/rdf-tests](https://github.com/w3c/rdf-tests) carries the RDF 1.1 and 1.2 syntax and
semantics suites and the SPARQL 1.0, 1.1 and 1.2 suites with their expectations rewritten for
[rdf-array](https://github.com/rdf-array/rdf-array), RDF whose data model has sequences as terms: a
collection `( … )` is one term, `()` the empty sequence, and `rdf:first`, `rdf:rest` and `rdf:nil`
are ordinary IRIs. Branch `rdf-sequences`, published at <https://rdf-array.github.io/rdf-tests-array/>.
Tests that are new under rdf-array, rather than derived from these, live in
[rdf-tests-contrib](https://github.com/rdf-array/rdf-tests-contrib).

## The rules

1. **A test is never deleted.** A rewritten test keeps its name, and its manifest entry gains an
   `rdfs:comment "rdf-array: …"` saying what changed and why.
2. **Expectations change, inputs do not.** A Turtle, TriG or RDF/XML document reads differently under
   rdf-array; the document stays as written and its expected N-Triples or N-Quads change. Those files
   are now N-Triples-array and N-Quads-array: a term may be `( e1 e2 … )`.
3. **Every rewrite is derived twice and must agree.** The W3C expectation is folded mechanically
   (`tools/rdfarray.py fold` in rdf-array/rdf-array: each `rdf:first`/`rdf:rest` ladder that ends in
   `rdf:nil` becomes a sequence), and an rdf-array parser reads the test's input independently
   (jena-array for Turtle, TriG, N-Triples and N-Quads; rdfxml-streaming-parser-array for RDF/XML).
   The rewrite is written only when the two are isomorphic, blank nodes inside sequences included.
4. **Explicit vocabulary stays.** Where an input uses `rdf:first`, `rdf:rest` or `rdf:nil` as ordinary
   IRIs (RDF/XML's `rdfms-rdf-names-use` tests), nothing is folded: those are ordinary triples.

`python3 tools/rewrite-rdf-syntax.py ~/checkouts/rdf-array/rdf-tests-array` (in rdf-array/rdf-array)
reruns the derivation and reports any test where the two readings disagree; it finds nothing left to
rewrite on this branch.

## What changed

### RDF syntax suites: 40 evaluation tests

Turtle and TriG (24): every collection test's expected result now has the sequence term. RDF/XML (16):
`rdf:parseType="Collection"` gives one sequence (three tests, including a reified and two annotated
collections, where the sequence appears in every place the list head did), and `rdf:li` is no longer
numbered into `rdf:_1`, `rdf:_2`, …: it stays the `rdf:li` property (twelve tests; explicit
`rdf:_n` properties in the same documents stay as written).

| Suite | Test | Expected result | Change |
|---|---|---|---|
| rdf11/rdf-trig | `blankNodePropertyList_containing_collection` | [blankNodePropertyList_containing_collection.nq](../rdf/rdf11/rdf-trig/blankNodePropertyList_containing_collection.nq) | collection → sequence |
| rdf11/rdf-trig | `collection_object` | [collection_object.nq](../rdf/rdf11/rdf-trig/collection_object.nq) | collection → sequence |
| rdf11/rdf-trig | `collection_subject` | [collection_subject.nq](../rdf/rdf11/rdf-trig/collection_subject.nq) | collection → sequence |
| rdf11/rdf-trig | `empty_collection` | [empty_collection.nq](../rdf/rdf11/rdf-trig/empty_collection.nq) | collection → sequence |
| rdf11/rdf-trig | `first` | [first.nq](../rdf/rdf11/rdf-trig/first.nq) | collection → sequence |
| rdf11/rdf-trig | `last` | [last.nq](../rdf/rdf11/rdf-trig/last.nq) | collection → sequence |
| rdf11/rdf-trig | `nested_collection` | [nested_collection.nq](../rdf/rdf11/rdf-trig/nested_collection.nq) | collection → sequence |
| rdf11/rdf-trig | `trig-subm-08` | [trig-subm-08.nq](../rdf/rdf11/rdf-trig/trig-subm-08.nq) | collection → sequence |
| rdf11/rdf-trig | `trig-subm-09` | [trig-subm-09.nq](../rdf/rdf11/rdf-trig/trig-subm-09.nq) | collection → sequence |
| rdf11/rdf-turtle | `blankNodePropertyList_containing_collection` | [blankNodePropertyList_containing_collection.nt](../rdf/rdf11/rdf-turtle/blankNodePropertyList_containing_collection.nt) | collection → sequence |
| rdf11/rdf-turtle | `collection_object` | [collection_object.nt](../rdf/rdf11/rdf-turtle/collection_object.nt) | collection → sequence |
| rdf11/rdf-turtle | `collection_subject` | [collection_subject.nt](../rdf/rdf11/rdf-turtle/collection_subject.nt) | collection → sequence |
| rdf11/rdf-turtle | `empty_collection` | [empty_collection.nt](../rdf/rdf11/rdf-turtle/empty_collection.nt) | collection → sequence |
| rdf11/rdf-turtle | `first` | [first.nt](../rdf/rdf11/rdf-turtle/first.nt) | collection → sequence |
| rdf11/rdf-turtle | `last` | [last.nt](../rdf/rdf11/rdf-turtle/last.nt) | collection → sequence |
| rdf11/rdf-turtle | `nested_collection` | [nested_collection.nt](../rdf/rdf11/rdf-turtle/nested_collection.nt) | collection → sequence |
| rdf11/rdf-turtle | `turtle-eval-lists-01` | [turtle-eval-lists-01.nt](../rdf/rdf11/rdf-turtle/turtle-eval-lists-01.nt) | collection → sequence |
| rdf11/rdf-turtle | `turtle-eval-lists-02` | [turtle-eval-lists-02.nt](../rdf/rdf11/rdf-turtle/turtle-eval-lists-02.nt) | collection → sequence |
| rdf11/rdf-turtle | `turtle-eval-lists-03` | [turtle-eval-lists-03.nt](../rdf/rdf11/rdf-turtle/turtle-eval-lists-03.nt) | collection → sequence |
| rdf11/rdf-turtle | `turtle-eval-lists-04` | [turtle-eval-lists-04.nt](../rdf/rdf11/rdf-turtle/turtle-eval-lists-04.nt) | collection → sequence |
| rdf11/rdf-turtle | `turtle-eval-lists-05` | [turtle-eval-lists-05.nt](../rdf/rdf11/rdf-turtle/turtle-eval-lists-05.nt) | collection → sequence |
| rdf11/rdf-turtle | `turtle-eval-lists-06` | [turtle-eval-lists-06.nt](../rdf/rdf11/rdf-turtle/turtle-eval-lists-06.nt) | collection → sequence |
| rdf11/rdf-turtle | `turtle-subm-08` | [turtle-subm-08.nt](../rdf/rdf11/rdf-turtle/turtle-subm-08.nt) | collection → sequence |
| rdf11/rdf-turtle | `turtle-subm-09` | [turtle-subm-09.nt](../rdf/rdf11/rdf-turtle/turtle-subm-09.nt) | collection → sequence |
| rdf11/rdf-xml | `rdf-containers-syntax-vs-schema-test001` | [test001.nt](../rdf/rdf11/rdf-xml/rdf-containers-syntax-vs-schema/test001.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdf-containers-syntax-vs-schema-test002` | [test002.nt](../rdf/rdf11/rdf-xml/rdf-containers-syntax-vs-schema/test002.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdf-containers-syntax-vs-schema-test003` | [test003.nt](../rdf/rdf11/rdf-xml/rdf-containers-syntax-vs-schema/test003.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdf-containers-syntax-vs-schema-test004` | [test004.nt](../rdf/rdf11/rdf-xml/rdf-containers-syntax-vs-schema/test004.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdf-containers-syntax-vs-schema-test007` | [test007.nt](../rdf/rdf11/rdf-xml/rdf-containers-syntax-vs-schema/test007.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdf-containers-syntax-vs-schema-test008` | [test008.nt](../rdf/rdf11/rdf-xml/rdf-containers-syntax-vs-schema/test008.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdf-ns-prefix-confusion-test0011` | [test0011.nt](../rdf/rdf11/rdf-xml/rdf-ns-prefix-confusion/test0011.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdf-ns-prefix-confusion-test0012` | [test0012.nt](../rdf/rdf11/rdf-xml/rdf-ns-prefix-confusion/test0012.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdf-ns-prefix-confusion-test0013` | [test0013.nt](../rdf/rdf11/rdf-xml/rdf-ns-prefix-confusion/test0013.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdf-ns-prefix-confusion-test0014` | [test0014.nt](../rdf/rdf11/rdf-xml/rdf-ns-prefix-confusion/test0014.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdfms-identity-anon-resources-test004` | [test004.nt](../rdf/rdf11/rdf-xml/rdfms-identity-anon-resources/test004.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdfms-rdf-names-use-test-031` | [test-031.nt](../rdf/rdf11/rdf-xml/rdfms-rdf-names-use/test-031.nt) | rdf:li kept |
| rdf11/rdf-xml | `rdfms-seq-representation-test001` | [test001.nt](../rdf/rdf11/rdf-xml/rdfms-seq-representation/test001.nt) | collection → sequence |
| rdf11/rdf-xml | `rdfms-seq-representation-test002` | [test002.nt](../rdf/rdf11/rdf-xml/rdfms-seq-representation/test002.nt) | collection → sequence |
| rdf12/rdf-xml | `rdf12-xml-an-15` | [rdf12-xml-an-15.nt](../rdf/rdf12/rdf-xml/eval/rdf12-xml-an-15.nt) | collection → sequence |
| rdf12/rdf-xml | `rdf12-xml-an-16` | [rdf12-xml-an-16.nt](../rdf/rdf12/rdf-xml/eval/rdf12-xml-an-16.nt) | collection → sequence |

### Unchanged on purpose

- **Syntax tests.** Positive and negative syntax tests keep their verdicts: `( … )` is still legal
  wherever it was, still illegal as a predicate or a graph name, and a sequence alone (`( 1 2 3 ) .`,
  `{ ( ) }`) is still not a triple.
- **`trig12-syntax-bad-06`** puts a collection inside a reified triple, which RDF 1.2 forbids because a
  collection generates triples. A sequence generates none, so rdf-array could allow it, but neither
  jena-array nor N3.js-array does today; it stays negative and is an open question in
  [rdf-array.md](https://rdf-array.github.io/rdf-array/rdf-array.html).
- **`rdfms-rdf-names-use-test-013/014/016/028/029`** use `rdf:first`, `rdf:rest`, `rdf:nil` as property
  names in RDF/XML; their expectations are ordinary triples.
- **RDF semantics** (`rdf11/rdf-mt`, `rdf12/rdf-semantics`): no test data uses collections or the list
  vocabulary.
- **SPARQL.** jena-array, a SPARQL-array engine, finds 31 query, update and data files with collections;
  in every evaluation test the data and the query both use collection syntax, so both sides become
  sequences and the results do not change. jena-array reproduces `sparql10/basic/list-1` to `list-4` and
  `sparql11/construct/constructlist` exactly. The OWL entailment tests (`sparql11/entailment/simple1`
  to `simple7`, `paper-sparqldl-Q2`) use OWL's list-valued properties on both sides; their meaning
  under rdf-array depends on an OWL reading of sequences that is not defined. `sparql11/property-path/pp05`
  walks a hand-written ladder with `rdf:rest{0,1}`, but no manifest lists it (the `{n,m}` path syntax
  left SPARQL 1.1), so it is not a test. No W3C SPARQL test walks `rdf:rest*/rdf:first`, so none needed
  the position patterns `((?e))[i]`; those are exercised in rdf-tests-contrib.
- **Manifests** keep `mf:entries ( … )`. Read with an rdf-array parser that list is one sequence term, so
  a test runner of an rdf-array implementation must accept a sequence there as well as a ladder.

## Using it

Point a runner at `https://rdf-array.github.io/rdf-tests-array/<path>/manifest.ttl` (or a local clone)
instead of `https://w3c.github.io/rdf-tests/<path>/manifest.ttl`. Each manifest's
`mf:assumedTestBase` still names the W3C location, so expected results resolve exactly as before.
