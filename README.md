# genomix

## Installation

pip install -r requirements.txt

### Get samtools

Samtools is being used for manipulation with genomic data.

TODO

## Storage

A variant file .vcf is diff file against reference genome, to keep files smaller.
There is also its binary version .bcf to keep the files even more compact.

### Get reference genome

Most of our sequencing data are generated against hg38 reference genome. We need to download this one.

    wget ftp://ftp.ensembl.org/pub/release-104/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz

Index it to make any subsequent operations faster.

    samtools faidx Homo_sapiens.GRCh38.dna.primary_assembly.fa

### Generate .bcf / .vcf file for diffs

Generate .bcf file with all variants kept.

    bcftools mpileup -Ou -f hg38.fa 8f0fd05f-3b35-422c-b5d4-bc9f2888225f.bam | \
    bcftools call -mv -Ob -o variants.bcf

Convert back to .vcf

    bcftools view variants.bcf -Oz -o variants.vcf.gz

Filter out low-quality data for even better compression

    bcftools filter -s LOWQUAL -e '%QUAL<20 || DP<10' variants.vcf -o filtered_variants.vcf
