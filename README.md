# E. coli REL606 NGS Variant Analysis

A reproducible bacterial next-generation sequencing (NGS) variant-calling and functional-annotation workflow using public sample **SRR2584863** relative to the *Escherichia coli* REL606 reference genome.

## Objective

Identify high-confidence variants relative to REL606, predict their functional consequences, and review read-level support for high-impact indels.

## Workflow

```text
Public sequencing reads (SRR2584863)
  → Reference-based alignment
  → Sorted and indexed BAM
  → Variant calling and filtering
  → Gene-context analysis with BEDTools
  → Custom SnpEff database construction
  → Functional annotation
  → Read-level frameshift QC with samtools mpileup
```

## Tools

- Linux / Bash
- Conda / Miniforge
- SAMtools
- BCFtools
- BEDTools
- SnpEff 5.3a
- OpenJDK 21

## Results

A total of **21 high-confidence variants** were identified relative to the REL606 reference genome.

| Variant class | Count |
|---|---:|
| Missense variants | 13 |
| Predicted high-impact frameshift variants | 3 |
| Intergenic variants | 5 |
| Total | 21 |

### Predicted high-impact frameshifts

| Gene | Position | Genomic variant | Predicted protein consequence |
|---|---:|---|---|
| `ybaL` | 473,901 | CCGC → CCGCGC | p.Ala469fs |
| `mdtD` | 3,901,455 | A → AC | p.Leu322fs |
| `tamB` | 4,431,393 | TGG → T | p.Gly1176fs |

The three predicted frameshift indels were reviewed using `samtools mpileup`. Each had consistent indel support at the expected genomic position, including support from both alignment orientation classes.

## Repository contents

| Path | Contents |
|---|---|
| `results/vcf/SRR2584863.variant_effects_primary.tsv` | One primary functional effect per final variant |
| `results/vcf/SRR2584863.snpeff.vcf.gz` | Full SnpEff-annotated VCF |
| `results/vcf/SRR2584863.snpeff.vcf.gz.tbi` | Tabix index for the annotated VCF |
| `results/vcf/SRR2584863.frameshift_pileup_qc.txt` | Read-level QC notes for the frameshift variants |
| `results/vcf/SRR2584863.frameshift_sites.bed` | Frameshift-site genomic intervals |
| `results/final_report/SRR2584863.analysis_manifest.txt` | Output inventory and software-version record |
| `results/final_report/SRR2584863.final_outputs.sha256` | SHA-256 checksums for selected final outputs |

## Interpretation

This analysis found 13 predicted missense variants, three predicted high-impact frameshift indels, and five intergenic variants in SRR2584863 relative to REL606. Functional effects are computational predictions; read-pileup review supports the sequence-level indel calls but does not experimentally prove altered gene function or phenotype.

## Limitations

- The analysis uses one public sequencing sample and a reference-based approach.
- The custom SnpEff database was constructed from a converted NCBI GFF3 annotation.
- Functional effects require experimental validation before making phenotype or loss-of-function claims.
- Raw FASTQ and BAM files are excluded because of size; summarized outputs and reproducibility records are included.

## Author

Pranavi Somula  
Independent bioinformatics portfolio project
