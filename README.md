# taskfarm-pbsdsh

This taskfarm script and environment module for PBS PRO is meant for one to run multiple independent tasks on one single and/or multiple nodes. Each task can use one core or multiple cores as per required. Each task is however limited to run within one node. 

This is particularly useful for users who would like to run multiple ML (machine learning) jobs on multiple GPUs that are on different computer nodes. Please note that the GPU jobs are expected to be embarrassingly parallel. 

## modules folder

This folder contains the module environment setup file (modules/taskfarm/pbsdsh/0.1). You can install "taskfarm" folder in the modules folder in any folders in your system, which is the $MODULESPATH.
Alternatively, you can e.g. install this in your $HOME directory and use if with the following command

module add $HOME/modules

To check how to use this module:

module help taskfarm/pbsdsh

## app folder

This folder contains the the actual script (app/taskfarm/pbsdsh/0.1/pbsdsh_taskfarm) doing the magic. It is currently assumed that the app folder is installed in or copied to $HOME. If you would like to change it, please update the file in modules/taskfarm/pbsdsh/0.1.

## To use this module:

In your PBS batch script, 

	module load taskfarm/pbsdsh
	pbsdsh $TFARM_PBSDSH {no of nodes} {full path to file containing tasks}

In the file containing the tasks. The commands to execute on each core should be in one line. Refer to the sample files in app/taskfarm/tasks.txt (or tasks48.txt).

As environment settings are not forwarded by pbsdsh, you will have to reload your modules environment on each line of the file.

E.g. of file
	
	module load python; cd $SCRATCH/abc; python mycode.py
	
	module load python; cd $SCRATCH/def; python mycode2.py
	
	module load python; cd $SCRATCH/hij; python mycode3.py
	

If you would like to use different number of cores for each task, simply do the following:

E.g. of file

	module load IntelMpi; cd $SCRATCH/abc; mpirun -np 4 mycode
	
	module load IntelMpi; cd $SCRATCH/def; mpirun -np 8 mycode2
	
NOTE: Each task is however limited to run within a single node, i.e. cannot run across nodes.

Runtime logs of this modules will be available in the working directory where the qsub command was executed 

($PBS_O_WORKDIR/.taskfarm/$PBS_JOBID.log)
