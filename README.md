# Evidence for Transcription and Horizontal Gene Transfer in Dipteran Germline-Restricted Chromosomes (2025)

### Overview

This directory contains all code for the pre-print "Evidence for Transcription and Horizontal Gene Transfer in Dipteran Germline-Restricted Chromosomes".

Riccardo G. Kyriacou1,†, Marion Herbette1, Robert B. Baird3, Katy M. Monteith1, Laura Ross1,†, Christina N. Hodson2

1 Institute of Ecology and Evolution, University of Edinburgh, UK 
2 University College London, UCL Department of Genetics, Evolution & Environment, UK
3 Whitehead Institute for Biomedical Science and Howard Hughes Medical Institute, Cambridge, Massachusetts, USA

†Corresponding authors: R.G.Kyriacou@sms.ed.ac.uk, Laura.Ross@ed.ac.uk 

It provides scripts, analyses, and outputs for RNA-seq mapping, intergenic mapping, GRC gene expression, and horizontal gene transfer analysis, ordered accordingly to the manuscript. README files provided for each subdirectory providing usage information for scripts. 

---

### Directory Summary

| Directory | Description |
|-----------|-------------|
| `01_RNAseq_Mapping` | Scripts to align RNA-seq reads to the reference _B. coprophila_ genome using STAR and calculate TPM values with StringTie. |
| `02_Intergenic_mapping` | Scripts to generate intergenic GFF files, map reads to intergenic regions, and calculate intergenic TPMs. |
| `03_Expressed_GRC_genes` | Scripts to identify expressed GRC-linked genes, BLAST GRC genes against core genome, and perform homology analysis against NCBI NR proteins and RepBase. |
| `04_HGT` | Scripts to investigate putative horizontal gene transfer (HGT) regions in GRC2. |
| `05_Urban_et_al_re-analysis` | Scripts to re-analyse pooled embryo RNA-seq data from Urban et al. (2021). |
| `figures` | Raw figures and code used to generate main paper Figures 1, 2 and 3 |
| `supplementary_materials` | Raw figures and code used to generate supplementary figures S2 and S4 and perform statistics |
---

### Directory Structure
```text
GRC_transcription/
├── 01_RNAseq_Mapping/
│   ├── 01_STAR_TPM_Nmax2
│   ├── 02_get_TPM_values.py
│   └── outputs/
│       ├── combined_TPM_only.tsv
│       ├── gene_overlap_0-4h_TPM.tsv
│       ├── gene_overlap_4-8h_TPM.tsv
│       ├── gene_overlap_adult_TPM.tsv
│       └── gene_overlap_late-larva-early-pupa_TPM.tsv
├── 02_Intergenic_mapping/
│   ├── 01_get_intergenic_GFF3.py
│   ├── 02_intergenic_TPM.sh
│   ├── 03_get_intergenic_TPM.py
│   ├── 04_intergenic_TPM_deconvolution.R
│   └── outputs/
│       ├── bcop_core_GRC.intergenic.gff3
│       ├── bcop_core_GRC.intergenic.gtf
│       ├── combined_intergenic_TPM.tsv
├── 03_Expressed_GRC_genes/
│   ├── 01_get_expressed_genes.py
│   ├── 02_BLAST_GRCgenes.sh
│   ├── 03_get_BLAST_table.py
│   ├── 04_get_GRC_proteins.py
│   ├── 05_BLAST_GRC_proteins.sh
│   ├── 06_get_interpro_summary.py
│   ├── 07_repbase_tBLASTn_GRCproteins.sh
│   └── outputs/
│       ├── GRC_BLAST_table.tsv
│       ├── GRC_gene_expression.tsv
│       └── GRC_gene_homology_BLAST/
│           ├── GRC_transcripts_BLAST_output.tsv
│           ├── GRCtranscript_repbase_output.tsv
│           ├── iprscan5_GRC_genes.tsv
│       └── GRC_v_Core_BLAST/
│           ├── GRC_v_Core_gene_BLAST_output
│           ├── GRC_v_Core_genome_BLAST_output.tsv
│       └── fasta_files/
│           ├── GRC_genes.nucl.fasta
│           ├── GRC_transcripts.fasta
├── 04_GRC2_HGT/
│   ├── 01_align_raw_reads_to_assembly.sh
│   ├── 02_BLAST_flanking_genes.sh
│   ├── 03_align_Rickettsia_assembly.sh
│   ├── 04_FastGA.sh
│   ├── 05_HGTregion_alignment.R
│   ├── 06_get_GC_content.py
│   ├── 07_sliding_window_GC.R
│   ├── get_kingdom.py
│   └── data/
│       ├── 290kb_HGT_only.fasta
│       ├── 651kb_HGT_region.fasta
│       ├── 651kb_length.tsv
│       ├── GC_290kb_HGT_region_only_w2000_n1000.tsv
│       ├── GC_651kb_full_HGT_region_w2000_n1000.tsv
│       ├── GC_651kb_full_HGT_region_w2000_n1000.tsv
│       ├── GC_SUPER_GRC2_w200000_n100000.tsv
│       ├── Rickettsiaceae_contig_sizes.tsv
│       ├── idBraCopr2.1.chrom_sizes.tsv
│   └── outputs/
│       ├── grc2_vs_rickettsia_1to1.1aln.paf
├── 05_Urban_et_al_re-analysis/
│   ├── 01_download_pooled_embryo.sh
│   ├── 02_STAR_Stringtie.sh
│   ├── 03_get_TPM_table.py
│   ├── Average_TPM.R
│   └── outputs/
│       ├── combined_TPM_Urban_embryo.tsv
│       ├── combined_TPM_Urban_embryo_GRC.tsv
├── figures/
│   ├── Figure1_mismapping.svg
│   ├── Figure2_gene_expression.svg
│   ├── Figure3_pie_expression.svg
│   ├── Figure4_HGT.svg
│   └── outputs/
│       ├── Figure_01.R
│       ├── Figure_02.R
│       ├── Figure_03.R
├── supplementary_materials/
│   ├── Figure_S2
│   ├── Figure_S3
└── README.md
```
