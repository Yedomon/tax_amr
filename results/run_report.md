# Taxon-aware AMR / virulence pipeline — final report

## Executive summary

- **Input rows:** 16
- **Bacterial assemblies analysed:** 3 (3 with AMR hits, 0 clean, 0 failed)
- **Non-bacterial assemblies (gene count only):** 9
- **Skipped rows** (no genome, suppressed, not found): 4
- **Top drug classes detected:** `BETA-LACTAM` (2), `MACROLIDE` (2), `AMINOGLYCOSIDE` (2), `ISONIAZID` (1), `RIFAMYCIN` (1)
- **Bacterial rows with virulence hits:** 3
- **Auto-replaced suppressed accessions:** 1
- **Downloaded this run:** 725.0 KB; **freed by cleanup:** 725.0 KB
- **Cache:** 12 successful summaries cached (2 failed, will re-try after TTL)

**Deliverables:**
- `results/final_output.tsv` — minimal schema (genome_id, total_gene_count, virulence_genes, virulence_gene_count, amr_genes, amr_gene_count)
- `results/final_output_full.tsv` — extended schema with all audit columns
- `results/decisions.tsv` — per-row decision log
- `results/run_report.md` — this document

### Final output preview

| genome_id | species | total_gene_count | amr_gene_count | virulence_gene_count |
|---|---|---:|---:|---:|
| `GCA_964341285.1` | Bacillus_anthracis | 5544 | 2 | 5 |
| `GCA_900654255.1` | Mycobacterium_tuberculosis | 4173 | 5 | 4 |
| `GCA_001049615.1` | Staphylococcus_aureus | 2872 | 6 | 8 |
| `GCA_000847345.1` | RVF_virus | 7 | _n/a_ | _n/a_ |
| `GCA_900327715.1` | Rabies_lyssavirus | 5 | _n/a_ | _n/a_ |
| `GCA_019455585.1` | Toxoplasma_gondii | 8920 | _n/a_ | _n/a_ |
| `GCA_035232765.1` | Cryptosporidium_parvum | 3865 | _n/a_ | _n/a_ |
| `GCA_014900015.1` | Schizophyllum_commune | 13210 | _n/a_ | _n/a_ |
| `GCA_964199775.1` | Zea_mays | 39591 | _n/a_ | _n/a_ |
| `GCA_030014295.1` | Loxodonta_africana | 22871 | _n/a_ | _n/a_ |
| `GCA_025407655.1` | Aedes_aegypti | 19500 | _n/a_ | _n/a_ |
| `none` | Cercocebus_galeritus |  |  |  |
| `none` | Kigelia_africana |  |  |  |
| `none` | Clavipitaceous_Fungi |  |  |  |
| `GCA_999999999.9` | Imaginary_species |  |  |  |
| `GCA_022413745.1` | Diceros_bicornis | 21845 | _n/a_ | _n/a_ |

---

# Taxon-aware AMR / virulence pipeline — run summary

- **Started**:  `2026-05-14T09:19:33+00:00`
- **Finished**: `2026-05-14T09:19:34+00:00`
- **Duration**: `0:00:01.049637`
- **Rows processed**: 16
- **Per-row TSV log**: `results/decisions.tsv`

## Outcome counts

| Status | Count | Meaning |
|---|---:|---|
| `ok_bacterial` | 3 | Bacterial assembly — AMRFinderPlus and VFDB will run. |
| `ok_genecount_only` | 9 | Eukaryotic or viral assembly — gene count only; bacterial AMR/VFDB databases are not biologically applicable. |
| `skipped_no_genome` | 3 | No genome accession in input — nothing to analyse. |
| `skipped_assembly_not_found` | 1 | Accession given but NCBI could not resolve it (suppressed / withdrawn / typo). |

## Resolved taxon groups

| Taxon group | Count |
|---|---:|
| `bacteria` | 3 |
| `virus` | 2 |
| `fungus` | 1 |
| `protozoa` | 2 |
| `plant` | 1 |
| `metazoa` | 3 |
| `unknown` | 4 |

## Bacterial rows (AMR + virulence will run): 3

| Input genome_id | NCBI accession | Organism | Assembly level |
|---|---|---|---|
| `GCA_964341285.1` | `GCA_964341285.1` | *Bacillus anthracis* | complete_genome |
| `GCA_900654255.1` | `GCA_900654255.1` | *Mycobacterium tuberculosis* | scaffold |
| `GCA_001049615.1` | `GCA_001049615.1` | *Staphylococcus aureus* | contig |

## Gene-count-only rows: 9

Bacterial AMR/VFDB **not run** for these — they are not bacterial. Gene counts will be derived from the canonical annotation.

| Genome | Organism | Group |
|---|---|---|
| `GCA_000847345.1` | *Rift Valley fever virus* | virus |
| `GCA_900327715.1` | *Rabies lyssavirus* | virus |
| `GCA_019455585.1` | *Toxoplasma gondii* | protozoa |
| `GCA_035232765.1` | *Cryptosporidium parvum* | protozoa |
| `GCA_014900015.1` | *Schizophyllum commune* | fungus |
| `GCA_964199775.1` | *Zea mays* | plant |
| `GCA_030014295.1` | *Loxodonta africana* | metazoa |
| `GCA_025407655.1` | *Aedes aegypti* | metazoa |
| `GCA_022413745.1` | *Diceros bicornis* | metazoa |

## Skipped rows: 4

| Input genome_id | Input species | Reason |
|---|---|---|
| `none` | Cercocebus_galeritus | No genome accession provided in input. |
| `none` | Kigelia_africana | No genome accession provided in input. |
| `none` | Clavipitaceous_Fungi | No genome accession provided in input. |
| `GCA_999999999.9` | Imaginary_species | NCBI assembly summary unavailable for GCA_999999999.9. |

## Audit warnings: 2

These rows were routed correctly from the NCBI lineage, but the input columns (`taxon_id` or `category`) disagreed with NCBI. Routing is unaffected; this is a data-quality signal for the input table.

- **Imaginary_species** (`GCA_999999999.9` → `None`)
    - Accession GCA_999999999.9 not resolvable at NCBI (suppressed / withdrawn / typo?).
    - NCBI status: not_found — Mock: accession not present in fixture set.
- **Diceros_bicornis** (`GCA_015501595.1` → `GCA_022413745.1`)
    - Auto-replaced suppressed accession: GCA_015501595.1 → GCA_022413745.1

## Method notes

- Routing key: **NCBI Assembly accession (GCA/GCF)**. Input `taxon_id` and `category` are audit fields only.
- AMR / virulence tools (AMRFinderPlus, VFDB) are run **only** on assemblies whose NCBI lineage contains `Bacteria`.
- Eukaryotic and viral assemblies receive `not_applicable` in the AMR / virulence columns, with the reason recorded per row.

## Step 2 — retrieval, caching, cleanup

### Per-row download plan

Files downloaded depend on what each row needs. Bacterial rows need genome FASTA + protein FASTA + GFF3 for AMRFinderPlus combined mode; non-bacterial rows whose gene count is already in the NCBI summary download nothing.

| Files retrieved | Rows |
|---|---:|
| _(none)_ | 13 |
| `genome+protein+gff3` | 3 |

### Disk usage

- Downloaded this run: **725.0 KB**
- Freed by cleanup: **725.0 KB** 
- Currently on disk: **0.0 B** across 0 files

### Cache health

- Summaries cached: **14** (12 ok, 2 failed)

### Suppressed-assembly substitutions

| Input accession | Final accession used | Species |
|---|---|---|
| `GCA_015501595.1` | `GCA_022413745.1` | Diceros_bicornis |

## Step 3 — total gene counts

| Source | Rows |
|---|---:|
| `ncbi_summary` | 11 |
| `—` | 4 |
| `gff3_parsed` | 1 |

- `ncbi_summary`: from cached `annotation_info.stats.gene_counts.total`.
- `gff3_parsed`: GFF3 features of type `gene` or `pseudogene`.

## Step 4 — AMR gene detection (AMRFinderPlus)

- Bacterial rows analysed: **3**
- Non-bacterial rows (AMR not applicable): **9**
- AMRFinderPlus invoked in **combined mode** (`-n -p -g`) for highest sensitivity. `--plus` is on, so stress + virulence rows are also in cache for Step 5.

### Per-row AMR profile

| Accession | Organism | --organism | Genes | Classes |
|---|---|---|---:|---|
| `GCA_964341285.1` | *Bacillus anthracis* | `_(none)_` | 2 | BETA-LACTAM; MACROLIDE |
|  |  |  |  | _Genes:_ blaA; msrA |
| `GCA_900654255.1` | *Mycobacterium tuberculosis* | `Mycobacterium_tuberculosis` | 5 | ISONIAZID; RIFAMYCIN; QUINOLONE; AMINOGLYCOSIDE |
|  |  |  |  | _Genes:_ katG_S315T; inhA; rpoB_S531L; gyrA_D94G; rrs_A1401G |
| `GCA_001049615.1` | *Staphylococcus aureus* | `Staphylococcus_aureus` | 6 | BETA-LACTAM; MACROLIDE; TETRACYCLINE; AMINOGLYCOSIDE |
|  |  |  |  | _Genes:_ mecA; mecR1; blaZ; ermC; tet(K); aac(6')-aph(2'') |

### Drug-class summary

| Drug class | Rows hit |
|---|---:|
| `BETA-LACTAM` | 2 |
| `MACROLIDE` | 2 |
| `AMINOGLYCOSIDE` | 2 |
| `ISONIAZID` | 1 |
| `RIFAMYCIN` | 1 |
| `QUINOLONE` | 1 |
| `TETRACYCLINE` | 1 |

## Step 5 — virulence gene detection

- Bacterial rows with virulence hits: **3**
- Bacterial rows clean (no virulence): **0**
- Non-bacterial rows (virulence not applicable): **9**
- Evidence merged from **two sources**: ABRicate-VFDB (curated virulence factor database) and AMRFinderPlus `--plus` virulence rows (already in cache from Step 4). Deduplicated by gene symbol.

### Per-row virulence profile

| Accession | Organism | Genes | Count | Sources |
|---|---|---|---:|---|
| `GCA_964341285.1` | *Bacillus anthracis* | pagA; lef; cya; capB; capC | 5 | amrfinderplus_plus; vfdb_abricate |
| `GCA_900654255.1` | *Mycobacterium tuberculosis* | esxA; esxB; eccCb1; mmpL7 | 4 | amrfinderplus_plus; vfdb_abricate |
| `GCA_001049615.1` | *Staphylococcus aureus* | hlb; clfA; clfB; spa; hla; sak; sea; icaA | 8 | amrfinderplus_plus; vfdb_abricate |

