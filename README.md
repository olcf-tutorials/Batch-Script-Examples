# Batch Script Examples
Copy-Paste ready!

### Summit
```
#!/bin/bash
# BEGIN LSF DIRECTIVES
#BSUB -P <PROJECT_ID>
#BSUB -J <JOB_NAME>
#BSUB -o <JOB_NAME>.o%J
#BSUB -e <JOB_NAME>.e%J
#BSUB -W <WALLTIME_HH:MM>
#BSUB -nnodes <NUMBER_OF_NODES>
# END LSF DIRECTIVES

number_of_nodes=$(echo $LSB_HOSTS | tr " " "\n" | grep -v "batch" | uniq | wc -l)

# JSRUN OPTIONS (CONFIGURE ME!)
number_of_resource_sets=12
resource_sets_per_node=6
physical_cores_per_resource_set=7
gpus_per_resource_set=1
mpi_ranks_per_resource_set=1
phyiscal_cores_per_mpi_rank=7

export OMP_NUM_THREADS=1

jsrun -n ${number_of_resource_sets}                   \
          -r ${resource_sets_per_node}                \
          -c ${physical_cores_per_resource_set}       \
          -g ${gpus_per_resource_set}                 \
          -a ${mpi_ranks_per_resource_set}            \
          -bpacked:${physical_cores_per_resource_set} \
           js_task_info |& sort -k2 -n
```

<hr>

***Notes:***
- `js_task_info |& sort -k2 -n` is merely a stand-in here, and should be replaced with your executable. 
- This script is intended to be a starting place. It may be sufficient for very simple runs, but only a few `jsrun` options are included here. For a more in-depth look at `jsrun`, see its very own [jsrun Quick Start Guide](https://github.com/olcf-tutorials/jsrun_quick_start_guide).
- If you're looking for other examples of job submission scripts for Summit, see [this great repo](https://github.com/dappelha/summit-scripts) by [@dappelha](https://github.com/dappelha)
- `number_of_nodes` is not used directly in this script. Since LSF doesn't provide an environment variable for it, this one-liner is a quick way to capture the number of compute nodes a job is requesting, and is included here as reference.

<hr>
