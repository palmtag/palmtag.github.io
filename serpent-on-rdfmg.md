# Running Serpent on the RDMFG Cluster

Last update: July 20, 2021

## SLURM

The RDFMG cluster uses SLURM to submit jobs.  
The SLURM commands start with `#SLURM` and an example is shown below.

```
#!/bin/bash
#SBATCH -p defq                 # default queue name
#SBATCH -J slpar64              # job name (change this to whatever you want)
#SBATCH -N 1                    # specify number of nodes to run
#SBATCH --ntasks-per-node=1     # specify number of tasks to run per node
#SBATCH --cpus-per-task=64
```

## Serpent

Serpent is a Monte Carlo code installed on RDFMG cluster.
You can run Serpent using either OpenMP (shared memory) or MPI (non-shared memory)

You need to first load gfortran version 7.2 to run Serpent
```
module load MPICH/3.2.1-GCC-7.2.0-2.29
```

## Example 1

The following run script run Serpent using OpenMP and 32 processors
```
#!/bin/bash
#SBATCH -p defq
#SBATCH -J thread32
#SBATCH -N 1                   # use one node with OpenMP
#SBATCH --ntasks-per-node=1    # use one task with OpenMP
#SBATCH --cpus-per-task=32     # specify number of processors (32)

cd $SLURM_SUBMIT_DIR
date

EXEC=/cm/shared/codes/serpent/bin/2.1.31/sss2
export SERPENT_ACELIB="/cm/shared/codes/serpent/xsdata/endfb71/sss_endfb71u.xsdata"

CASE=NRX_Test_Set     # name of input file

# Note that an environment variable was used to specify the number of processors
# to the same specified on the slurm command above (32)
$EXEC -omp $SLURM_JOB_CPUS_PER_NODE $CASE.serp
```


