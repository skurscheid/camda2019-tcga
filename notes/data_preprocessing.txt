trinity does not seem to like the FASTQ headers from the TCGA files.
currently investigating if it is the missing 1/2 at the end of the header that causes the issue (these are used to indicate which read of the pair it is)
attempting to fix with:
# zcat 000837e3-cc9f-4226-ba38-44fa45140671_1.fastq.gz | awk '{print (NR%4 == 1) ? $0 " 1" : $0}' | gzip -c > 000837e3-cc9f-4226-ba38-44fa45140671_1.fixed.fastq.gz
