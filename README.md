# Topiary
Predict mutation-derived cancer T-cell epitopes from (1) somatic variants (2) tumor RNA expression data, and (3) patient HLA type.

## Example

```sh
./topiary \
  --vcf somatic.vcf \
  --mhc-pan \
  --mhc-alleles HLA-A*02:01,HLA-B*07:02 \
  --ic50-cutoff 500 \
  --percentile-cutoff 2.0 \
  --mhc-epitope-lengths 8-11 \
  --rna-gene-fpkm-file genes.fpkm_tracking \
  --rna-min-gene-expression 4.0 \
  --rna-transcript-fpkm-file isoforms.fpkm_tracking \
  --rna-min-transcript-expression 1.5 \
  --output-csv epitopes.csv \
  --output-html epitopes.html
```

## Commandline Arguments

### Genomic Variants

You must specify at least one variant input file:

* `--vcf VCF_FILENAME`: Load a [VCF](http://www.1000genomes.org/wiki/analysis/variant%20call%20format/vcf-variant-call-format-version-41) file
* `--maf MAF_FILENAME`: Load a TCGA [MAF](https://wiki.nci.nih.gov/display/TCGA/Mutation+Annotation+Format+%28MAF%29+Specification) file

### Output Format

* `--output-csv OUTPUT_CSV_FILENAME`: Path to an output CSV file
* `--output-html OUTPUT_HTML_FILENAME`: Path to an output HTML file

### RNA Expression Filtering

Optional flags to use Cufflinks expression estimates for dropping epitopes
arising from genes or transcripts that are not highly expressed.

* `--rna-gene-fpkm-file RNA_GENE_FPKM_FILE`: Cufflinks FPKM tracking file
containing gene expression estimates.
* `--rna-min-gene-expression RNA_MIN_GENE_EXPRESSION`: Minimum FPKM for genes
* `--rna-remap-novel-genes-onto-ensembl`: If a novel gene is fully contained
within a known Ensembl gene, remap its FPKM expression value on the Ensembl
gene ID.
* `--rna-transcript-fpkm-file RNA_TRANSCRIPT_FPKM_FILE`: Cufflinks FPKM tracking
file containing transcript expression estimates.
* `--rna-min-transcript-expression RNA_MIN_TRANSCRIPT_EXPRESSION`: Minimum FPKM
for transcripts

### Choose an MHC Binding Predictor

You *must* choose an MHC binding predictor using one of the following flags:
* `--mhc-pan`: Local NetMHCpan
* `--mhc-cons`: Local NetMHCcons
* `--mhc-random`: Random IC50 values
* `--mhc-smm`: Local SMM
* `--mhc-smm-pmbec`: Local SMM-PMBEC
* `--mhc-pan-iedb`: NetMHCpan via the IEDB web API
* `--mhc-cons-iedb`: NetMHCcons via the IEDB web API
* `--mhc-smm-iedb`: SMM via the IEDB web API
* `--mhc-smm-pmbec-iedb`: SMM-PMBEC via the IEDB web API

### MHC alleles
You must specify the alleles to perform binding prediction for using one of
the following flags:

* `--mhc-alleles-file MHC_ALLELES_FILE`: Text file containing one allele name per
line
* `--mhc-alleles MHC_ALLELES`: Comma separated list of allele names,
e.g. "HLA-A02:01,HLA-B07:02"

### MHC Binding Options
* `--mhc-epitope-lengths MHC_EPITOPE_LENGTHS`: comma separated list of integers
specifying which peptide lengths to perform MHC binding prediction for

### Misc
* `--padding-around-mutation PADDING_AROUND_MUTATION`
* `--self-filter-directory SELF_FILTER_DIRECTORY`
* `--skip-variant-errors`
