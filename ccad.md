### Running on DCEG CCAD cluster (SGE)


#### Prepare the pipeline

- activate the snakemake conda environment created in the installation step 

```conda activate snakemake```

- clone the repository

modify ```config/samples.txt``` and ```config/config.yaml``` as described in [here](inputs_and_outputs.md)


- go to workflow directory

```cd <cloned repo dir>/workflow```

- create a script that looks like this

```
#!/bin/bash

module load singularity

cd <cloned repo dir>/workflow

snakemake  --profile profiles/ccad --use-singularity --singularity-args "--bind /DCEG" --jobs 100 --latency-wait 300 --rerun-incomplete
```

#### submit the script 

```qsub -q long.q -V -j y -cwd -o ${PWD} <script>```

#### monitor the jobs

```qstat```

The log files are in ```<cloned repo dir>/workflow/logs/```, it is also useful to look at ```<cloned repo dir>/workflow/<script>.o<jobid>``` for snakemake logs

#### Fine tuning the pipeline

- To change the queue a particular rule is being submit to, you can edit the _profiles/ccad/cluster.yaml_ file. For example, adding
```
HC_call_variants :
     queue : bigmem.q
```     
to the _profiles/ccad/cluster.yaml_ file tells GEMSCAN to submit the  _HC_call_variants_ jobs to bigmem.q 	