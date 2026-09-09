# GlasAgent AP 1.2.2 – Glass Ontology simulation release candidate 2.0.0-rc1

**Project:** GlasAgent (Platform MaterialDigital), https://www.material-digital.de/project/28
**Work package:** AP 1.2.2 – representation of computer simulations in the glass ontology
**Responsible team:** Friedrich Schiller University Jena, Otto Schott Institute of Materials Research, Sierka Computational Materials Science Group
**Technical ontology contact:** Azin Mazloom Jalali
**Package status:** presentation-stage release candidate (2.0.0-rc1), prepared for stakeholder presentation and feedback
**Licence:** CC BY 4.0 – see [LICENSE_AND_ATTRIBUTION.md](LICENSE_AND_ATTRIBUTION.md)

## 1 Background

The Glass Ontology (GO) was originally developed in the **GlasDigital** project as an ontology-based
digital infrastructure for data-driven glass development. GlasDigital is the historical predecessor
of the current project. The ontology is now being **extended in GlasAgent**. This package is the
AP 1.2.2 contribution of that extension: it adds a simulation module to the Glass Ontology so that
computer simulations of glasses can be described in the same knowledge structure as glass compositions,
properties, manufacturing processes and analysis procedures.

## 2 What this release candidate covers

The simulation module covers **simulation methods and workflows**, in particular:

- atomistic glass simulations (for example melt-quench molecular dynamics and analytical property
  evaluation), interatomic potentials and simulation software;
- data-driven glass property prediction and machine-learning potentials, including the training
  processes and training datasets behind them;
- thermochemical simulation (phase-equilibrium and devitrification-kinetics calculations,
  thermodynamic models and CALPHAD-type assessment) as a minimal branch that still awaits
  domain-expert validation;
- simulation protocols, workflow definitions, step ordering, parameter specifications and
  simulated property data;
- links between glass simulations and the manufacturing and analysis processes already present in
  the Glass Ontology.

The new classes use the identifier block `GO_0000300`–`GO_0000320` under the namespace
`https://w3id.org/pmd/glass-ontology/`. The module reuses existing terms from the Glass Ontology
baseline, the PMD Core Ontology (PMDco), BFO, OBI, IAO, RO, STATO and NFDIcore wherever a suitable
term exists; no new object properties were introduced.

## 3 Package contents

| File | Role |
|---|---|
| `go_v2_candidate.ttl` | Self-contained release-candidate ontology (Turtle): the Glass Ontology baseline merged with the AP 1.2.2 simulation module and one additive repair of the baseline viscosity declaration. Ontology IRI `https://w3id.org/pmd/go.owl`, version IRI `https://w3id.org/pmd/go.owl/2.0.0-rc1`. 11,284 triples. |
| `abox_meltquench_example.ttl` | Synthetic demonstration ABox (Turtle): 61 illustrative individuals, 252 triples, in the reserved example namespace `https://example.org/glasagent/ap122/demo#`. |
| `cq/CQ1.rq` … `cq/CQ8.rq` | The eight competency questions as SPARQL 1.1 SELECT queries. Each file's header comment states the question, the instance path used and the expected answer columns. |
| `DATASPACE_NOTES.md` | Data-space notes: delivery status, what to load and how, competency-question smoke test, licence and contact, open items. |
| `LICENSE_AND_ATTRIBUTION.md` | Licence, attribution and reuse notices. |
| `SHA256SUMS` | SHA-256 checksums of every other file in this package (13 entries). |

### The ABox is synthetic demonstration content

`abox_meltquench_example.ttl` exists only to show how the ontology is meant to be used and to make
the competency questions executable. Every individual in it is synthetic and illustrative. The file
contains **no real measurement dataset, no production dataset and no simulation-result dataset**.
Where a numeric value or an entity such as a software release is named, it is a demonstration
fixture, not a research result. Individuals of the thermochemical branch are deliberately kept as
value-free skeletons.

### Notes on the query files

Only the publication-facing header comments of the eight query files (the comment block before the
first `PREFIX` line) were written for this package. The executable SPARQL content of every query,
from its first `PREFIX` line to the end of the file, is identical to the reviewed source. Each header
states the competency question, the instance path, the graph assumption, the expected result columns,
the expected row count on this package and any scientifically meaningful limitation.

## 4 How to load and query the package

`go_v2_candidate.ttl` and `abox_meltquench_example.ttl` are meant to be loaded **together as two RDF
inputs**, for example as two named graphs of a triple store whose default graph is the union of both:

1. `https://w3id.org/pmd/go.owl/2.0.0-rc1` ← `go_v2_candidate.ttl`
2. `https://example.org/glasagent/ap122/demo` ← `abox_meltquench_example.ttl` (label it as
   synthetic demonstration content in the store's catalogue)

**No OWL reasoning is required** to reproduce the recorded answers. The queries operate on the
asserted graph and traverse the asserted class hierarchy with `rdfs:subClassOf*`. A reasoner may
widen some answer sets; the recorded counts below refer to the asserted union only.

### Executable commands (Apache Jena, run from this directory)

Verify the files first:

```bash
sha256sum -c SHA256SUMS
```

Check that both files parse (Apache Jena `riot`):

```bash
riot --validate go_v2_candidate.ttl
riot --validate abox_meltquench_example.ttl
```

Run one competency question (Apache Jena `arq`), here CQ1:

```bash
arq --data go_v2_candidate.ttl --data abox_meltquench_example.ttl --query cq/CQ1.rq --results=CSV
```

Run all eight and print the number of result rows for each:

```bash
for n in 1 2 3 4 5 6 7 8; do
  printf 'CQ%s rows: ' "$n"
  arq --data go_v2_candidate.ttl --data abox_meltquench_example.ttl --query "cq/CQ$n.rq" --results=CSV | tail -n +2 | wc -l
done
```

### Alternative with RDFLib (Python)

```bash
python3 -c '
import rdflib, pathlib
g = rdflib.Graph()
g.parse("go_v2_candidate.ttl", format="turtle")
g.parse("abox_meltquench_example.ttl", format="turtle")
for n in range(1, 9):
    rows = list(g.query(pathlib.Path(f"cq/CQ{n}.rq").read_text(encoding="utf-8")))
    print(f"CQ{n} rows: {len(rows)}")
'
```

## 5 Expected competency-question results

Both files parse without errors (Apache Jena 5.6.0 `riot`; RDFLib 7.6.0). Running CQ1–CQ8 on the
asserted union of the two files returns exactly the following numbers of result rows:

| CQ | Question (short form) | Rows |
|---|---|---|
| CQ1 | Which simulation processes, of which simulation family, can determine a given glass property? | 3 |
| CQ2 | Which computational model is a specified input of which simulation? | 4 |
| CQ3 | What are the ordered steps of the melt-quench protocol, with their ensemble and parameter specifications? | 21 |
| CQ4 | Which simulation software executed which simulation? | 2 |
| CQ5 | Which simulated property data were validated against which assay-produced measurement data? | 1 |
| CQ6 | Which manufacturing or analysis process classes are connected to a glass simulation, and through which path? | 4 |
| CQ7 | Which training datasets, dataset-generating processes and training process stand behind a data-driven model? | 4 |
| CQ8 | Which thermodynamic model does a thermochemical simulation use, and which measured data entered its assessment? | 1 |

Expected row counts in one line: **3 / 4 / 21 / 2 / 1 / 4 / 4 / 1**.

Contract highlights: CQ1 returns the atomistic melt-quench rows for density and viscosity and the
data-driven row for density; CQ3 returns the eight protocol steps with their pairwise ordering and
one row per parameter specification; CQ4 returns the LAMMPS row for the melt-quench run under both
of its asserted process types and, by design, no row for the analytical property evaluation;
CQ5 returns exactly one validation with full assay provenance; CQ6 returns link paths 1–3 (path 2
twice); CQ7 returns both training chains with the simulation-dataset versus other-dataset contrast;
CQ8 returns the single value-free thermochemical skeleton.

## 6 Relationship to this repository and next steps

- This package is placed in an isolated, versioned directory on purpose. The reviewed candidate
  and the current `v2-dev` editor state of this repository were produced from different baseline
  states of the Glass Ontology, so the candidate cannot be dropped into the ODK editor file
  without a reconciliation step. No canonical ontology product or ODK workflow file of this
  repository is changed by this package.
- **Canonical ODK integration** of the simulation module (editor file, imports, release products)
  will be undertaken only **after stakeholder feedback** on this release candidate has been
  collected and evaluated.
- **SciGlass Next runtime deployment** of the ontology is **deferred** until after that feedback.
- The thermochemical branch (phase-equilibrium calculation, devitrification-kinetics simulation,
  CALPHAD assessment, thermodynamic model) is a minimal placeholder branch pending validation by
  the designated domain experts.

## 7 Provenance

- Source repository and reviewed commit:
  https://github.com/AzinMazloomJalali/Simulation/commit/a450424004a1da42a8e046a8e0948d29ab4c15a4
- `go_v2_candidate.ttl` and `abox_meltquench_example.ttl` are byte-identical copies of
  `AP_1.2.2/go_v2_candidate.ttl` and `AP_1.2.2/abox_meltquench_example.ttl` at that commit.
- `cq/CQ1.rq`–`cq/CQ8.rq`: the executable portion of each query, from its first `PREFIX` line to
  the end of the file, is byte-identical to `AP_1.2.2/cq/CQ1.rq`–`CQ8.rq` at that commit. Only the
  header comments before the first `PREFIX` line were replaced by publication-facing headers.
- SHA-256 of `go_v2_candidate.ttl`: `f1cda6cffc879eb6679cbbf7342d9b68ea160f25f59b2f72c6bede9c886ec562`
  (all checksums: `SHA256SUMS`).
- The candidate header records the merge inputs (baseline `go.ttl`, module
  `https://w3id.org/pmd/glass-ontology/ap122-simulation` version 0.1.0, additive repair) with their
  SHA-256 values, and the merge tool (ROBOT 1.9.10). Tooling used for the recorded results:
  Apache Jena 5.6.0 (`riot`, `arq`), RDFLib 7.6.0, Temurin JRE 17.
