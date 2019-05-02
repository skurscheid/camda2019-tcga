---
Title: Execution of snakemake workflows on NCI's raijin
output: markdown
---

* Notes regarding execution of snakemake workflows on NCI's raijin
** Using the swith "--use-conda" in conjunction with "--create-envs-only" allows one time execution of a job on copyq as this will only create the necessary conda environment
** After a snakemake process has been terminated due to exceeding walltime, it is necessary to use --unlock before re-submission
