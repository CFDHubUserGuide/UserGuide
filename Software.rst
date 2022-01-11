.. _Software:

===================
SOFTWARE
===================

The software installed on the cluster can be installed standalone or installed with modules.

A not-comprehensive list of available software is:

- :ref:`Abaqus<Abaqus>`
- :ref:`Ansys Fluent<AnsysFluent>`
- :ref:`Comsol<Comsol>`
- :ref:`FDS<FDS>`
- :ref:`Matlab<Matlab>`
- :ref:`OpenFOAM<OpenFOAM>`
- :ref:`Python<PythonSection>`

Other software and software versions may be found through *modules* or in the directory ``/software``.



.. _Abaqus:

-------------------------
Abaqus
-------------------------

TBD


.. _AnsysFluent:

-------------------------
Ansys Fluent
-------------------------

TBD


.. _Comsol:

-------------------------
Comsol
-------------------------

TBD


.. _FDS:

-------------------------
FDS
-------------------------

TBD


.. _Matlab:

-------------------------
Matlab
-------------------------

`Matlab Software <https://it.mathworks.com>`_ is available on CFDHub.

The software is installed in ``/software/MATLAB``. With ``ls`` in the folder is possible to check which versions are available.

::

    [<username>@nodevg-0-1 ~]$ cd /software/MATLAB
    [<username>@nodevg-0-1 MATLAB]$ ls

*If you are asking for more than one cpu, please make sure your script will use all requested cpus.*

You can use the software in different ways:

- :ref:`batch job on queues<MatlabBatch>`;
- :ref:`interactive job on queues<MatlabInteractive>`;
- :ref:`interactive job on node<MatlabNode>`.


.. _MatlabBatch:

Batch job using queues
---------------------------

To submit a Matlab job using queues, prepare the launch file ``matlabJob.sh`` that will be used to run your script. Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``queues`` you have access to.

The result of the computation will be written on file (please make sure to save all relevant variables). The output will be written on the *jobOutput* file. if you wish you may redirect in another file (``myMatlabOutput``).

*In this case you will not have the graphical interface, so make sure your script does not open any figure, otherwise its execution will stop with errors.*

*If you are asking for more than one cpu, please make sure your script will use all requested cpus.*

::

    #!/bin.bash             # use bash as command interpreter
    #$ -cwd                 # currentWorkingDirectory
    #$ -N myMatlabJob       # jobName
    #$ -j y                 # merges output and errors
    #$ -S /bin/bash         # scripting language
    #$ -l walltime=1:00:00  # jobDuration hh:mm:ss
    #$ -q hub.q             # queueName
    #$ -pe mpi 2            # cpuNumber
    #________________________________________________________
    
    ### Runs the Matlab "script.m" file.
    ### You may change it to launch your script.
    ### 
    ### Change R2018a with your desired version

    /software/MATLAB/R2018a/bin/matlab -noDesktop -nosplash -r "script"
    # /software/MATLAB/R2018a/bin/matlab -noDesktop -nosplash -r "script" >& myMatlabOutput
    
    ### You may run also a second script
    ### or another software in the same job.
    
    /software/MATLAB/R2018a/bin/matlab -noDesktop -nosplash -r "script2"
    
    echo End Parallel Run

To launch your ``matlabJob.sh`` file you may execute:

``[<username>@nodevg-0-1 jobDirectory]$ qsub matlabJob.sh``

To check how the job is proceeding from the login node, reading the output, you may use:

``[<username>@nodevg-0-1 jobDirectory]$ tail -f myMatlabOutput``



.. _MatlabInteractive:

Interactive job using queues
--------------------------------

To submit an interactive Matlab job using queues, you need to ask one or more cpus to the desired queue. Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``queues`` you have access to.

*In this case you will not have the graphical interface, so make sure your script does not open any figure, otherwise its execution will stop with errors.*

*If you are asking for more than one cpu, please make sure your script will use all requested cpus.*

You need to follow the :ref:`instructions to launch an interactive job on a queue<InteractiveQueue>`. Be sure to be logged in your login node (``nodevg-0-1`` or ``nodevg-0-2``) and ask to the Job Scheduler your resources:

``qrsh -q hub.q -l h_rt=2:00:00 -l h_vmem=2G -pe mpi 2``

These instructions request on the *hub.q* queue (``-q hub.q``) 2 cpus (``-pe mpi 2``) for 2 hours (``-l h_rt=2:00:00``, hh:mm:ss) and 2GB of RAM (``-l h_vmem=2G``).
You can adjust the request according to your need.

As reported in the :ref:`instructions to launch an interactive job<InteractiveQueue>` you may prepare an alias as well.


The result of the computation will be written on file (please make sure to save all relevant variables). The output will be written on the *jobOutput* file. if you wish you may redirect in another file (``myMatlabOutput``).

You will be then redirected on a node, ready to start your computation:

::

    [<username>@nodevg-0-1 ~]$ qrsh -q hub.q -l h_rt=2:00:00 -l h_vmem=2G -pe mpi 2
    ... wait for node assigment
    [<username>@<node> ~]$
    ... node assigned
    [<username>@<node> ~]$ cd myScriptDir
    [<username>@<node> myScriptDir]$ /software/MATLAB/R2018a/bin/matlab -noDesktop -nosplash -r "script"
    [<username>@<node> myScriptDir]$ /software/MATLAB/R2018a/bin/matlab -noDesktop -nosplash -r "script" >& myMatlabOutput &

You will be running the script "script.m" using Matlab R2018a.

To check how the job is proceeding when writing the output to file you may use:

``[<username>@<node> myScriptDir]$ tail -f myMatlabOutput``


.. _MatlabNode:

Interactive job on a node
-------------------------------

To submit an interactive Matlab job on a node, you need to login on a node. Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``nodes`` you have access to.

*If you are asking for more than one cpu, please make sure your script will use all requested cpus.*

You need to follow the :ref:`instructions to launch an interactive job on a node<InteractiveNode>`. Be sure to be logged in your login node (``nodevg-0-1`` or ``nodevg-0-1``) and check if your desired node is free.

Once you logged in a node, you may run your script:

::

    [<username>@nodevg-0-1 ~]$ ssh <node>
    [<username>@nodevg-0-1 ~]$ cd myFolder
    [<username>@<node> myFolder]$ /software/MATLAB/R2018a/bin/matlab -noDesktop -nosplash -r "script"``






.. _OpenFOAM:

-------------------------
OpenFOAM
-------------------------

OpenFOAM is available on CFDHub.

All three OpenFOAM versions are available:

- `OpenFOAM by ESI <https://www.openfoam.com>`_;
- `FOAM-Extend Project <https://foam-extend.sourceforge.io>`_;
- `OpenFOAM-Foundation <https://openfoam.org>`_;

The software is installed in ``/software/OpenFOAM``. With ``ls`` in the folder is possible to check which versions are available.

::

    [<username>@nodevg-0-1 ~]$ cd /software/OpenFOAM
    [<username>@nodevg-0-1 OpenFOAM]$ ls

To load a version of OpenFOAM, it is necessary to source the ``bashrc`` file of the chosen version (or to include its sourcing in your ``~/.bashrc`` file as you would do on your personal pc) in order to load the OpenFOAM environment.

::

    [<username>@nodevg-0-1 ~]$ source /software/OpenFOAM/OpenFOAM-8/etc/bashrc

It is also possibile to use OpenFOAM through modules, using the relevant module (``module use ...``), looking for the relevant version (``module avail``) and loading the choosen version (``module load ...``):

::

    [<username>@nodevg-0-1 ~]$ module use /big-scratch/software/modules/mecc4/CFD
    [<username>@nodevg-0-1 ~]$ module avail
    [<username>@nodevg-0-1 ~]$ module load openfoam-v2106

To check that you correctly loaded OpenFOAM, you can run the following command, verifying that the system recognizes the solver (*simpleFOAM* is available for all OpenFOAM versions) and it will tell you where it is located (to check that the correct version of OpenFOAM is loaded, *OpenFOAM-8* in this case):

::

    [<username>@nodevg-0-1 ~]$ which simpleFoam
    /software/OpenFOAM/OpenFOAM-8/platforms/linux64GccDPInt32Opt/bin/simpleFoam

*If you require to launch a job with many cpus please verify the scalability of your simulation (OpenFOAM generally scales well up to 100.000 cells per core), but please verify your setup. Since the cluster is used by many users please check the availability of cpus.*

You can use the software in different ways:

- :ref:`batch job on queues<OpenFOAMBatch>`;
- :ref:`interactive job on queues<OpenFOAMInteractive>`;
- :ref:`interactive job on node<OpenFOAMNode>`.







.. _OpenFOAMBatch:

Batch job using queues
---------------------------

To submit a OpenFOAM job using queues, prepare the launch file ``OpenFOAMJob.sh`` that will be used to run your script. Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``queues`` you have access to.

The result of the computation will be written on file according to what you specified in your ``system/controlDict`` file. The output will be written on the *jobOutput* file. if you wish you may redirect in another file (typically ``log.$solver``).

*If you are asking for more than one cpu, please make sure your requested cpus and the number of *processors* are coincident, so you will use all requested cpus.*

Here an example of launch file:

::

    #!/bin.bash             # use bash as command interpreter
    #$ -cwd                 # currentWorkingDirectory
    #$ -N myOpenFOAMJob     # jobName
    #$ -j y                 # merges output and errors
    #$ -S /bin/bash         # scripting language
    #$ -l walltime=3:00:00  # jobDuration hh:mm:ss
    #$ -q hub.q             # queueName
    #$ -pe mpi 4            # cpuNumber
    #---------------------------------------------------------
    
    ### LOAD THE OPENFOAM ENVIRONMENT
    source /software/OpenFOAM/OpenFOAM-8/etc/bashrc
    
    #module use /big-scratch/software/modules/mecc4/CFD
    #module load openfoam-v2106
    
    #---------------------------------------------------------
    
    ### EXECUTE COMMANDS
    #./Allrun
    
    blockMesh >& log.blockMesh
    decomposePar >& log.snappyHexMesh
    mpirun snappyHexMesh -parallel >& log.snappyHexMesh
    mpirun simpleFoam -parallel >& log.simpleFoam
    reconstructPar -latestTime >& log.reconstructPar
    sample -latestTime >& log.sample

    echo End Parallel Run

as you may see you can either choose to load the OpenFOAM environment using modules or sourcing the *bashrc* file. Just add/remove *hashtags* [#] to comment/uncomment the lines. To execute the commands, you may either include an executable file (``Allrun`` in this case), or list all relevant commands.

To launch your ``OpenFOAMJob.sh`` file from the *login node*, from the ``jobDirectory`` you may execute:

``[<username>@nodevg-0-x jobDirectory]$ qsub OpenFOAMJob.sh``

To check the status of the job you may use the ``qstat -u <username>`` command to see if the job started. To check how the job is proceeding from the login node, reading the output, you may use:

``[<username>@nodevg-0-1 jobDirectory]$ tail -f log.simpleFoam``







.. _OpenFOAMInteractive:

Interactive job using queues
--------------------------------

To submit an interactive OpenFOAM job using queues, you need to ask one or more cpus to the desired queue. Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``queues`` you have access to.

*If you are asking for more than one cpu, please make sure your script will use all requested cpus.*

You need to follow the :ref:`instructions to launch an interactive job on a queue<InteractiveQueue>`. Be sure to be logged in your login node (``nodevg-0-1`` or ``nodevg-0-2``) and ask to the Job Scheduler your resources:

``qrsh -q hub.q -l h_rt=2:00:00 -l h_vmem=2G -pe mpi 2``

These instructions request on the *hub.q* queue (``-q hub.q``) 2 cpus (``-pe mpi 2``) for 2 hours (``-l h_rt=2:00:00``, hh:mm:ss) and 2GB of RAM (``-l h_vmem=2G``).
You can adjust the request according to your need.

As reported in the :ref:`instructions to launch an interactive job<InteractiveQueue>` you may prepare an alias as well.

To make an interactive OpenFOAM job you will need to ask some computational resources ``qrsh -q ...``, load the OpenFOAM environment sourcing the bashrc or loading the module (eventually verifying that everything works correctly ``which simpleFoam``) and then start with the interactive job:

::

    [<username>@nodevg-0-1 ~]$ qrsh -q hub.q -l h_rt=2:00:00 -l h_vmem=2G -pe mpi 2
    ... wait for node assigment
    [<username>@<node> ~]$
    ... node assigned
    [<username>@<node> ~]$ source /software/OpenFOAM/OpenFOAM-8/etc/bashrc
    # [<username>@<node> ~]$ module use /big-scratch/software/modules/mecc4/CFD
    # [<username>@<node> ~]$ module load openfoam-v2106
    [<username>@<node> ~]$ which simpleFoam
    /software/OpenFOAM/OpenFOAM-8/platforms/linux64GccDPInt32Opt/bin/simpleFoam
    [<username>@<node> ~]$ cd myJobFolder
    [<username>@<node> myJobFolder]$ blockMesh
    [<username>@<node> myScriptDir]$ blockMesh >& log.blockMesh &
    [<username>@<node> myScriptDir]$ tail -f log.blockMesh

You will be running *blockMesh* using *OpenFOAM-8*.

Two ways of running are reported: in the first you will see what the solver is foreground; in the second the solver will run in background (see tailing ``&``) writing to file the output.





.. _OpenFOAMNode:

Interactive job on a node
-------------------------------

To submit an interactive OpenFOAM job on a node, you need to login on a node. Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``nodes`` you have access to.

You need to follow the :ref:`instructions to launch an interactive job on a node<InteractiveNode>`. Be sure to be logged in your login node (``nodevg-0-1`` or ``nodevg-0-1``) and check if your desired node is free.

Once you logged in a node, load the OpenFOAM environment sourcing the bashrc or loading the module (eventually verifying that everything works correctly ``which simpleFoam``) and then start with the interactive job:

::

    [<username>@nodevg-0-1 ~]$ ssh <node>
    [<username>@<node> ~]$ source /software/OpenFOAM/OpenFOAM-8/etc/bashrc
    # [<username>@<node> ~]$ module use /big-scratch/software/modules/mecc4/CFD
    # [<username>@<node> ~]$ module load openfoam-v2106
    [<username>@<node> ~]$ which simpleFoam
    /software/OpenFOAM/OpenFOAM-8/platforms/linux64GccDPInt32Opt/bin/simpleFoam
    [<username>@<node> ~]$ cd myJobFolder
    [<username>@<node> myJobFolder]$ blockMesh
    [<username>@<node> myScriptDir]$ blockMesh >& log.blockMesh &
    [<username>@<node> myScriptDir]$ tail -f log.blockMesh

You will be running *blockMesh* using *OpenFOAM-8*.

Two ways of running are reported: in the first you will see what the solver is foreground; in the second the solver will run in background (see tailing ``&``) writing to file the output.







.. _PythonSection:

-------------------------
Python
-------------------------


`Python <https://www.python.org>`_ is available on CFDHub.

The software is available in Linux OS.

You can use the software in different ways:

- :ref:`batch job on queues<PythonBatch>`;
- :ref:`interactive job on queues<PythonInteractive>`;
- :ref:`interactive job on node<PythonNode>`.


.. _PythonBatch:

Batch job using queues
---------------------------

To submit a Python job using queues, prepare the launch file ``PythonJob.sh`` that will be used to run your script. Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``queues`` you have access to.

The result of the computation will be written on file (please make sure to save all relevant variables). The output will be written on the *jobOutput* file. if you wish you may redirect in another file (``myPythonOutput``).

*If you are asking for more than one cpu, please make sure your requested cpus and the number of *processors* are coincident, so you will use all requested cpus.*

Here an example of launch file:

::

    #!/bin.bash             # use bash as command interpreter
    #$ -cwd                 # currentWorkingDirectory
    #$ -N myPythonJob       # jobName
    #$ -j y                 # merges output and errors
    #$ -S /bin/bash         # scripting language
    #$ -l walltime=2:00:00  # jobDuration hh:mm:ss
    #$ -q hub.q             # queueName
    #$ -pe mpi 2            # cpuNumber
    #---------------------------------------------------------
    
    ### EXECUTE COMMANDS
    python myPythonScript >& myPythonOutput
    
    echo End Parallel Run




.. _PythonInteractive:

Interactive job using queues
--------------------------------

To submit an interactive Python job using queues, you need to ask one or more cpus to the desired queue. Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``queues`` you have access to.

You need to follow the :ref:`instructions to launch an interactive job on a queue<InteractiveQueue>`. Be sure to be logged in your login node (``nodevg-0-1`` or ``nodevg-0-2``) and ask to the Job Scheduler your resources:

``qrsh -q hub.q -l h_rt=2:00:00 -l h_vmem=2G -pe mpi 2``

These instructions request on the *hub.q* queue (``-q hub.q``) 2 cpus (``-pe mpi 2``) for 2 hours (``-l h_rt=2:00:00``, hh:mm:ss) and 2GB of RAM (``-l h_vmem=2G``).
You can adjust the request according to your need.

As reported in the :ref:`instructions to launch an interactive job<InteractiveQueue>` you may prepare an alias as well.

To make an interactive Python job you will need to ask some computational resources ``qrsh -q ...``, and then start with the interactive job which may be with a script or directly writing commands:

::

    [<username>@nodevg-0-1 ~]$ qrsh -q hub.q -l h_rt=2:00:00 -l h_vmem=2G -pe mpi 2
    ... wait for node assigment
    [<username>@<node> ~]$
    ... node assigned
    [<username>@<node> ~]$ cd myJobFolder
    [<username>@<node> myJobFolder]$ python myPythonScript.py
    [<username>@<node> ~]$ python
    Python 2.6.6 (r266:84292, Nov 22 2013, 12:16:22)
    [GCC 4.4.7 20120313 (Red Hat 4.4.7-4)] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>>


Two ways of running are reported: in the first you are running your *myPythonScript*; in the second you are writing the instructions to python.



.. _PythonNode:

Interactive job on a node
--------------------------------

To submit an interactive OpenFOAM job on a node, you need to login on a node. Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``nodes`` you have access to.

You need to follow the :ref:`instructions to launch an interactive job on a node<InteractiveNode>`. Be sure to be logged in your login node (``nodevg-0-1`` or ``nodevg-0-1``) and check if your desired node is free.

To make an interactive Python job you will need to ask some computational resources ``qrsh -q ...``, and then start with the interactive job which may be with a script or directly writing commands:

::

    [<username>@nodevg-0-1 ~]$ ssh <node>
    [<username>@<node> ~]$ cd myJobFolder
    [<username>@<node> myJobFolder]$ python myPythonScript.py
    [<username>@<node> ~]$ python
    Python 2.6.6 (r266:84292, Nov 22 2013, 12:16:22)
    [GCC 4.4.7 20120313 (Red Hat 4.4.7-4)] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>>


Two ways of running are reported: in the first you are running your *myPythonScript*; in the second you are writing the instructions to python.