# Licence and attribution – GlasAgent AP 1.2.2 release candidate 2.0.0-rc1

## 1 Licence

The contents of this package that were created in the GlasAgent project – the AP 1.2.2 simulation
module contained in `go_v2_candidate.ttl`, the synthetic demonstration ABox
`abox_meltquench_example.ttl`, the competency-question queries `cq/CQ1.rq`–`cq/CQ8.rq`, and the
accompanying documentation (`README.md`, this file) – are released under the

**Creative Commons Attribution 4.0 International licence (CC BY 4.0)**
https://creativecommons.org/licenses/by/4.0/

The Glass Ontology baseline merged into `go_v2_candidate.ttl` declares the same licence in its
ontology header (`dcterms:license <https://creativecommons.org/licenses/by/4.0/>`).

## 2 Attribution

When reusing or redistributing this package or parts of it, please use the following attribution:

> Azin Mazloom Jalali, Friedrich Schiller University Jena, GlasAgent AP 1.2.2.

Responsible team: Friedrich Schiller University Jena, Otto Schott Institute of Materials Research,
Sierka Computational Materials Science Group. GlasAgent is a project of Platform MaterialDigital,
https://www.material-digital.de/project/28.

The Glass Ontology baseline records its creators in its ontology header (`dcterms:creator`):
Azin Mazloom Jalali, Hossein Beygi Nasrabadi, Simon Stier and Ya-Fan Chen. The baseline was
originally developed in the GlasDigital project. Please keep these header statements intact when
redistributing the ontology file.

## 3 Synthetic demonstration content

`abox_meltquench_example.ttl` is **synthetic demonstration content**. Every individual in it is
illustrative and was created only to exercise the ontology and the competency questions. It
contains no real measurement dataset, no production dataset and no simulation-result dataset.
Numeric values, named software releases and process descriptions in the ABox are demonstration
fixtures and must not be cited as scientific results.

## 4 Reused ontologies and third-party notices

`go_v2_candidate.ttl` reuses terms from other ontologies. Each reused term keeps the licence of its
originating ontology; this package does not change those licences. The file carries in-line source
annotations (`obo:IAO_0000119`, "definition source") for the terms whose labels or definitions were
copied verbatim.

- **PMD Core Ontology (PMDco)**, namespace `https://w3id.org/pmd/co/` – Platform MaterialDigital.
- **Basic Formal Ontology (BFO)**, **Ontology for Biomedical Investigations (OBI)**,
  **Information Artifact Ontology (IAO)**, **Relation Ontology (RO)**, **STATO** and **COB**,
  namespace `http://purl.obolibrary.org/obo/` – OBO Foundry ontologies, each under its own licence.
  The two IAO annotation properties introduced by the AP 1.2.2 module (`IAO_0000114`,
  `IAO_0000117`) carry a source annotation naming IAO release v2026-03-30 with the SHA-256 of the
  source file; label and definition were copied verbatim.
- **NFDIcore**, namespace `https://nfdi.fiz-karlsruhe.de/ontology/` – the class `NFDI_0001048`
  "simulation software" is reused directly with a source annotation naming NFDIcore v3.0.5 and the
  SHA-256 of the source file; its definition was copied verbatim.
- The baseline ontology header states that its initial terminology and definitions were derived
  mainly from the **SciGlass** database (https://github.com/epam/SciGlass), the GlasDigital project,
  robotic melting and analysis workflows of the Bundesanstalt für Materialforschung und -prüfung
  (BAM), and PMD Core Ontology concepts.

## 5 Trademarks and independently licensed source materials

- Names of software and organisations that appear in the ABox or in ontology annotations (for
  example LAMMPS, GULP, SciGlass, GTT-Technologies products, SCHOTT, BAM) are trademarks or names of
  their respective owners. They are mentioned for identification only. **No trademark is
  relicensed** by this package, and no endorsement is implied.
- Comments in the ABox point to a doctoral thesis and to project documents as the scientific
  anchors of the illustrative fixtures. Those source materials are independently licensed and are
  **not relicensed** here; only the synthetic individuals and their annotations in
  `abox_meltquench_example.ttl` are part of this package.
- Nothing in this file grants rights beyond those given by CC BY 4.0 for the GlasAgent-created
  content and by the respective original licences for reused material.
