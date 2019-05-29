# Implementation of "Best Practices for De Novo Transcriptome Assembly with Trinity"
## Source 
The original document can be found at https://informatics.fas.harvard.edu/best-practices-for-de-novo-transcriptome-assembly-with-trinity.html

Here I am addressing the points raised and document the implementation of the recommended steps in our CAMDA2019 TCGA pipeline.

## Remarks and comments about the steps outlined in the document:
* 2 Examine quality metrics for sequencing reads:

    Currently implemented with fastp which combines adapter trimming and quality assessment of read data. Needs to be reviewed in order to improve stringency, in particular regarding minimum read quality.

* 3 Removing erroneous k-mers from Illumina paired-end reads

    This step might incur an unacceptable performance hit, in particular as the software Rcorrector seems to only handle uncompressed fastq data.

* 4 Discard read pairs for which one of the reads is deemed unfixable

    Accordingly this step will not be implemented

* 5 Trim adapter and low quality bases from fastq files

    see above, implemented using fastp

* 6 Map trimmed reads to a blacklist to remove unwanted (rRNA reads) -- OPTIONAL

    An optional step, but in my opinion quite crucial in particular when dealing with a large volume of samples of unknown provenance.
    In order to reduce the search space, I am only including metazoan rRNA sequences:
    
    0. Download *Parc FASTA files from SILVA database:
    https://www.arb-silva.de/no_cache/download/archive/release_132/Exports/
    
    1. Filter SSU sequence IDs
    ```
    zcat SILVA_132_SSUParc_tax_silva.fasta.gz | grep '>' | grep 'Metazoa' | cut -f 1 -d " " | awk '{ gsub(">", "") ; print $0 }' > ssu_metazoa_IDs.txt
    ```
    2. Filter LSU sequence IDs
    ```
    zcat SILVA_132_LSUParc_tax_silva.fasta.gz | grep '>' | grep 'Metazoa' | cut -f 1 -d " " | awk '{ gsub(">", "") ; print $0 }' > lsu_metazoa_IDs.txt
    ```
    3. Index FASTA files using samtools
        
        1. convert gz to bgz (blocked for indexing)
        ```
            gzip -d -c SILVA_132_SSUParc_tax_silva.fasta.gz | pbgzip -c -n 2 - >  SILVA_132_SSUParc_tax_silva.fasta.bgz
            gzip -d -c SILVA_132_LSUParc_tax_silva.fasta.gz | pbgzip -c -n 2 - >  SILVA_132_LSUParc_tax_silva.fasta.bgz
        ```
        

