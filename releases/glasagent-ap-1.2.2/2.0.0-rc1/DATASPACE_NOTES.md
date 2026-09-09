# Data-space notes – GlasAgent AP 1.2.2 release candidate 2.0.0-rc1

These notes describe how the AP 1.2.2 ontology-side contribution is delivered and how it is meant to be
loaded into a data space or triple store. They accompany the presentation-stage release candidate
described in [README.md](README.md).

## 1 Status and delivery channel

- This package is a **public, presentation-stage GlasAgent release candidate**. It is distributed
  through GitHub pull request #11 of the `materialdigital/glasdigital-ontology` repository
  (https://github.com/materialdigital/glasdigital-ontology/pull/11), targeting the `v2-dev` branch.
- The pull request is **not merged**. The **immediate delivery channel is this versioned GitHub
  package** (`releases/glasagent-ap-1.2.2/2.0.0-rc1/`) on the pull-request branch.
- **Canonical ODK integration** of the simulation module into the editor file, imports and release
  products of this repository remains **deferred until after stakeholder feedback** has been
  collected and evaluated.
- **Physical SciGlass Next deployment** of the ontology remains **deferred**.
- **No physical data-space recipient, runtime operator or endpoint is activated by this pull
  request.** The designation of a deployment recipient and operator is an open project decision
  that will be taken after the stakeholder review.

## 2 Responsibility and contact

- **Technical ontology owner and contact:** Azin Mazloom Jalali.
- **Responsible team:** Friedrich Schiller University Jena, Otto Schott Institute of Materials
  Research, Sierka Computational Materials Science Group.
- Project: GlasAgent (Platform MaterialDigital), https://www.material-digital.de/project/28.
  Please use the issue tracker of this repository for questions and feedback on the release candidate.

## 3 What to load

The package contains two RDF files that belong together:

| Graph | File (package-relative path) | Content |
|---|---|---|
| `https://w3id.org/pmd/go.owl/2.0.0-rc1` | `go_v2_candidate.ttl` | Self-contained release-candidate ontology: Glass Ontology baseline merged with the AP 1.2.2 simulation module and one additive repair of the baseline viscosity declaration. 11,284 triples. |
| `https://example.org/glasagent/ap122/demo` | `abox_meltquench_example.ttl` | Synthetic demonstration ABox, 61 illustrative individuals, 252 triples. |

Load both files **together as the asserted union** – for example as two named graphs whose union
forms the default graph, or as two inputs of one in-memory model. The competency questions assume
this union. Verify each file against `SHA256SUMS` before loading:

```bash
sha256sum -c SHA256SUMS
```

The ABox is synthetic demonstration content. Label the demonstration graph accordingly in the
store's catalogue. It contains no real measurement, production or simulation-result data.

**No OWL reasoning is needed** to reproduce the recorded competency-question answers. All queries
operate on the asserted graph and traverse the asserted class hierarchy with `rdfs:subClassOf*`.
If reasoning is wanted for other purposes, the candidate lies inside OWL 2 DL but outside OWL 2 EL,
so an OWL 2 DL reasoner such as HermiT should be used, not ELK.

### Not reproducible from this package

The modular verification configuration used during development (the baseline `go.ttl`, the
separate simulation module file and the separate repair file loaded as individual graphs) is **not
executable from this minimal package**, because those separate files are not included here. The
header of `go_v2_candidate.ttl` records the SHA-256 checksums of the merge inputs for provenance.
The merged candidate is the artifact to load.

## 4 Competency-question smoke test

Run from this directory after loading or directly over the two files with Apache Jena `arq`:

```bash
arq --data go_v2_candidate.ttl --data abox_meltquench_example.ttl --query cq/CQ1.rq --results=CSV
```

Expected: exit code 0 and 3 result rows – (GO_0000301, GO_0000302, PMD_0000597),
(GO_0000301, GO_0000302, GO_0000046) and (GO_0000305, GO_0000305, PMD_0000597).

All eight queries:

```bash
for n in 1 2 3 4 5 6 7 8; do
  printf 'CQ%s rows: ' "$n"
  arq --data go_v2_candidate.ttl --data abox_meltquench_example.ttl --query "cq/CQ$n.rq" --results=CSV | tail -n +2 | wc -l
done
```

Expected row counts for CQ1–CQ8 on the asserted union: **3 / 4 / 21 / 2 / 1 / 4 / 4 / 1**. Each
query file states its question, graph assumption, expected result columns and expected row count in
its header comment. The same counts are obtained with RDFLib (see `README.md`, section 4).

Tool versions used for the recorded results: Apache Jena 5.6.0 (`riot`, `arq`), RDFLib 7.6.0,
Temurin JRE 17.

## 5 Licence

The GlasAgent-created content of this package is released under **CC BY 4.0**
(https://creativecommons.org/licenses/by/4.0/). The **synthetic ABox is authorised for public
distribution under CC BY 4.0** with the attribution

> Azin Mazloom Jalali, Friedrich Schiller University Jena, GlasAgent AP 1.2.2.

Reused ontology terms keep the licences of their originating ontologies. Details and third-party
notices: [LICENSE_AND_ATTRIBUTION.md](LICENSE_AND_ATTRIBUTION.md).

## 6 Open items after the stakeholder review

- Designation of the data-space recipient, runtime operator and endpoint.
- Canonical ODK integration, including reconciliation of the candidate baseline with the current
  `v2-dev` editor state.
- SciGlass Next runtime deployment.
- Validation of the thermochemical branch by the designated domain experts.
