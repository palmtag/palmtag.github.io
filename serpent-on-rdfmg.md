# Running Serpent on the RDMFG Cluster

Last update: July 20, 2021

## SLURM

The RDFMG cluster uses SLURM to submit jobs.  
The SLURM commands start with `#SLURM`.  Full examples are given below.

```
#!/bin/bash
#SBATCH -p defq                 # default queue name
#SBATCH -J slpar64              # job name (change this to whatever you want)
#SBATCH -N 1                    # specify number of nodes to run
#SBATCH --ntasks-per-node=1     # specify number of tasks to run per node
#SBATCH --cpus-per-task=64
```

You submit a job with the `sbatch` command.  For example, if your run script was called `run.sh`, 
you would submit to the queue with the command.
```
sbatch < run.sh
```


## Serpent

Serpent is a Monte Carlo code installed on RDFMG cluster.
You can run Serpent using either OpenMP (shared memory) or MPI (non-shared memory)

You need to first load gfortran version 7.2 to run Serpent and MPICH
```
module load GCC/7.2.0-2.29
module load MPICH/3.2.1-GCC-7.2.0-2.29
```

## Example 1 - OMP

The following run script run Serpent using OpenMP and 32 processors.
There is only one executable, so there is only one "task" in SLURM.

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

## Example 2 - MPI

The following run script run Serpent using MPI and 32 processors.
Each instance of MPI is a "task" for SLURM.

```
#!/bin/bash
#SBATCH -p defq
#SBATCH -J mpi32
#SBATCH -N 1                   # use one (or more) nodes with MPI
#SBATCH --ntasks-per-node=32   # choose number of MPI tasks to run per node
#SBATCH --cpus-per-task=1      # use one cpu per task with MPI

cd $SLURM_SUBMIT_DIR
date

EXEC=/cm/shared/codes/serpent/bin/2.1.31/sss2
export SERPENT_ACELIB="/cm/shared/codes/serpent/xsdata/endfb71/sss_endfb71u.xsdata"

CASE=NRX_Test_Set     # name of input file

# Note that an environment variable was used to specify the number of processors
# to the same specified on the slurm command above (32)
# This only works if one node is used, otherwise would have to use number explicitly
mpirun -np $SLURM_JOB_CPUS_PER_NODE $EXEC $CASE.serp
```

## Example 3 - Combine OMP and MPI

The following run script run Serpent using 4 MPI tasks and 16 processors per task (64 total).
Each instance of MPI is a "task" for SLURM.
Note that you can use multiple nodes if you want to add more cores.

```
#!/bin/bash
#SBATCH -p defq
#SBATCH -J combo64
#SBATCH -N 1                   # use one (or more) nodes with MPI
#SBATCH --ntasks-per-node=4    # choose number of MPI tasks to run per node
#SBATCH --cpus-per-task=16     # use one cpu per task with MPI

cd $SLURM_SUBMIT_DIR
date

EXEC=/cm/shared/codes/serpent/bin/2.1.31/sss2
export SERPENT_ACELIB="/cm/shared/codes/serpent/xsdata/endfb71/sss_endfb71u.xsdata"

CASE=NRX_Test_Set     # name of input file

# Specify the number of MPI tasks and number of OMP cores per task
mpirun -np 4 $EXEC -omp 16 $CASE.serp
```
