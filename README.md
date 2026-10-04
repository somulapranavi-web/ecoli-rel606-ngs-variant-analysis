# E. coli REL606 NGS Variant Analysis

This is an independent bioinformatics project in which I analysed a public *Escherichia coli* sequencing sample, **SRR2584863**, against the REL606 reference genome.

The aim was to complete a practical end-to-end NGS variant-analysis workflow: align reads to a reference genome, identify and filter variants, predict their functional effects, and manually inspect the strongest indel calls using read-level evidence.

## Project overview

This project follows the workflow below:

```text
Public sequencing reads (SRR2584863)
        ↓
Reference-based alignment to E. coli REL606
        ↓
Sorted and indexed BAM
        ↓
Variant calling and filtering
        ↓
Gene-context analysis with BEDTools
        ↓
Custom SnpEff database construction
        ↓
Functional variant annotation
        ↓
Read-level QC of frameshift indels using samtools mpileup
```

## Tools used

- Linux / Bash
- Conda / Miniforge
- SAMtools
- BCFtools
- BEDTools
- SnpEff 5.3a
- OpenJDK 21

## What I found

After filtering, I retained **21 high-confidence variants** relative to the *E. coli* REL606 reference genome.

| Variant type | Count |
|---|---:|
| Missense variants | 13 |
| Predicted frameshift variants | 3 |
| Intergenic variants | 5 |
| **Total** | **21** |

### Predicted frameshift variants

| Gene | Genomic position | Variant | Predicted protein effect |
|---|---:|---|---|
| `ybaL` | 473,901 | CCGC → CCGCGC | p.Ala469fs |
| `mdtD` | 3,901,455 | A → AC | p.Leu322fs |
| `tamB` | 4,431,393 | TGG → T | p.Gly1176fs |

The three predicted frameshift indels were manually reviewed using `samtools mpileup`.

- The `ybaL` site showed repeated support for the same 2-bp insertion.
- The `mdtD` site showed consistent support for the same 1-bp insertion among reads passing quality filters.
- The `tamB` site showed repeated support for the same 2-bp deletion, including the expected deletion placeholders at the deleted bases.

For all three sites, support was observed in both alignment orientation classes. This supports the presence of these indels in the sequencing data.

## What this project demonstrates

Through this project, I practised:

- Working with common genomics file formats: FASTA, GFF3, BAM, BAI, VCF, BED, and TSV.
- Reference-based alignment, variant calling, filtering, compression, and indexing.
- Genomic feature overlap analysis with BEDTools.
- Building a custom SnpEff database from a bacterial reference annotation.
- Predicting coding and noncoding effects of variants.
- Interpreting missense, frameshift, and intergenic variants.
- Reviewing high-impact indels at the read level using SAMtools.
- Recording final outputs and checksums for reproducibility.

## Key output files

| File | Description |
|---|---|
| `results/vcf/SRR2584863.variant_effects_primary.tsv` | One primary functional annotation for each of the 21 final variants |
| `results/vcf/SRR2584863.snpeff.vcf.gz` | Full SnpEff-annotated VCF |
| `results/vcf/SRR2584863.snpeff.vcf.gz.tbi` | Tabix index for the annotated VCF |
| `results/vcf/SRR2584863.frameshift_pileup_qc.txt` | Manual read-level QC notes for the three predicted frameshifts |
| `results/vcf/SRR2584863.frameshift_sites.bed` | BED intervals for the three frameshift sites |
| `results/final_report/SRR2584863.analysis_manifest.txt` | Analysis summary, software versions, and output inventory |
| `results/final_report/SRR2584863.final_outputs.sha256` | SHA-256 checksums for selected final outputs |

## Notes and limitations

This is a public-data workflow project and is not presented as a novel biological discovery.

The reported missense and frameshift effects are computational predictions based on a custom SnpEff database constructed from the REL606 reference annotation. The `samtools mpileup` review provides read-level support for the three indels, but it does not experimentally prove their effects on protein function, bacterial fitness, or phenotype.

Raw FASTQ reads and the alignment BAM are not included because of file size. The repository includes the final annotated VCF, primary-effect table, frameshift QC notes, reproducibility manifest, and file checksums.

## Author

**Pranavi Somula**  
Independent Bioinformatics Portfolio Project
