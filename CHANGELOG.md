# Changelog

All notable changes to homolocus are recorded here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [0.1.0] - 2026-11-20

The first release. Thresholds and score weights are unvalidated starting values; see section 18 of the specification.

### Added

- **Inputs:**
  - reference regions from GenBank, as a whole genome with a contig and 1-based coordinates, or as a region-only file;
  - target assemblies from GenBank or FASTA, with the format detected from content and each genome named after its file;
  - Pyrodigal gene prediction for FASTA and for GenBank without CDS (`auto` or `pyrodigal` annotation);
  - CDS translation rules, with skipped pseudogenes, internal stops and compound CDS recorded in the run metadata.
- **Search:**
  - DIAMOND protein search (2.0 or later) that keeps every hit and HSP, aggregated per gene pair with union-based coverage;
  - paralogous reference genes grouped into families from a DIAMOND self-search.
- **Family names:** families are named from the reference annotation, using their genes' `/gene` qualifiers, else informative `/product` values. Products such as "hypothetical protein" don't name a family. The name is in `GeneFamily.name`, `FamilyEvidence.family_name` and the candidate TSVs' `family_name` column.
- **Detection:**
  - candidate regions found per contig from chains and windows of matching events, tolerant of missing and inserted genes, duplicated homologues, rearranged order and reversed regions;
  - a weighted score of content, similarity, order, orientation and spacing, with every component exposed;
  - flags for split proteins, fused proteins, partial genes and candidates at a contig edge;
  - each candidate called `forward` or `reversed` relative to the reference (gene order, then strand), in `Cluster.reversed`.
- **Several target genomes:**
  - `--genome` takes one or more files, and `RegionFinder.find_all()` returns `GenomeSetResults`;
  - families and the self-search are computed once, and each genome's results equal a run on it alone;
  - `--names FILE` (file name, tab, presentation name) sets the genomes' shown names and order, also available as `Genome.display_name` and `Genome.label`.
- **Results:** `RegionResults` with ranked candidates, `to_dataframe()` and `genes_dataframe()`.
- **Output:**
  - a deterministic output directory with `summary.tsv`, per-candidate TSVs, raw DIAMOND tables, `analysis.json` and PNG plots;
  - `summary.tsv` includes `direction` and `missing_families`;
  - several genomes get a folder each (named after the file), a combined `summary.tsv` with a `genome` column, and a combined overview plot.
- **Plots:**
  - an overview, a per-candidate plot and a combined multi-genome overview;
  - every candidate is drawn in the reference direction, and a reversed one sits on a descending axis marked `reversed`;
  - family colours are shared across plots, gene arrows are labelled by `/gene` name (else family ID) on two alternating rows, and the legend wraps to fit;
  - matched genes fade with the identity of their match (full at 100 %, half at `--min-identity`), with an identity key.
- **Command line:** `homolocus find`, with every setting as a flag, exit codes 0, 1 and 2, and a stdout summary.
- **Requirements:** Python 3.10–3.13. Tested minimum versions are Biopython 1.80, pandas 2.0, Matplotlib 3.8 and Pyrodigal 3.0.

[Unreleased]: https://github.com/emilhaegglund/homolocus/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/emilhaegglund/homolocus/releases/tag/v0.1.0
