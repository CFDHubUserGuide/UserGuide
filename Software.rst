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
- :ref:`Spack<SpackSection>`

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

This section presents how to start and configure Ansys Fluent on the cluster. The user is expected to have some experience with this CFD code since this user guide covers how to access Ansys Fluent on a single or multiple nodes. 

Launch Fluent from Terminal
--------------------------------

Before launching Ansys Fluent is necessary to enter in the node where the user want to run the simulation.

.. ( nella vecchia guida aggiungeva un “ , as explained in section 4.1 and 4.2”; rimetterlo? Se si, aggiornare a che capitoli a cui fa riferimento)

After the user has logged from the terminal to the node, should go into the folder where the case to be run is located. From this folder the user should launch the command:

    ``/software/ansys_inc/v212/fluent/bin/fluent``
    
    .. ``/fluent``
    
.. ( se si userà l’approccio dei moduli stile cineca, aggiornare mettendo il solo comando e non il path del comando, verificare che sia questo il comando ) 

Parallel run on Single Node set-up 
--------------------------------

If the user wants to make a parallel run just with the processors of a single node, Ansys Fluent should be configurated as follows.
In *Parallel Settings* tab, the voice **Shared Memory On Local Machine** should be selected under the the voice *Run Types*, as can be seen in the following figure:

.. figure:: images/single-node.png

At the voice *Solver Processes* the user could also select how many cores wants to use.

Parallel run on Multiple Nodes set-up
--------------------------------

If the user wants to make a parallel run with the processors of multiple nodes, Ansys Fluent should be configurated as follows.
In *Parallel Settings* tab, the voice **Distributed Memory On a Cluster** should be selected under the the voice *Run Types*, as can be seen in the next figure.
When **Distributed Memory On a Cluster** is selected, new options appears. Selecting **Machine Names** it is possible to enlist the nodes that the user wants to use, as can be seen in the following figure:

.. figure:: images/multiple-node.png

At the voice *Solver Processes* the user could also select how many cores wants to use.
When not explicited, the cores will be equally distributed on the multiple node.
The user can choose how to distribute the cores on the nodes changing from *node-x-y, node-x-z* in figure with *node-x-y:16, node-x-z:16* assunming that the user is working with a total of *32* processors.

Parallel CPU+GPU run set-up
--------------------------------

Management of crashed processes
--------------------------------

When the user starts Ansys Fluent, in the working directory (by default the *path* where you are launching Ansys Fluent) a script named *cleanup-fluent`...`.sh* is created.
If for various reasons Ansys Fluent crashes, typically giving an error message in red color, the Fluent processes could not stop or leave residual processes running. 
You can check it with the *top* or *htop* command. 
In order to clean this processes the user can use the .sh file previously created by launching:
    ``sh cleanup-fluent`...`.sh``
When Ansys Fluent is correctly stopped, the script *cleanup-fluent`...`.sh* is automatically deleted.
This procedure is advised everytime that this script doesn't automatically disappear after closing Ansys Fluent.

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
- :ref:`interactive job on node<MatlabNode>`;
- :ref:`interactive job on multiple nodes<MatlabMultipleNodes>`;


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
    #$ -l h_rt=1:00:00      # jobDuration hh:mm:ss
    #$ -q hub.q             # queueName
    #$ -pe mpi 2            # cpuNumber
    #________________________________________________________
    
    ### Runs the Matlab "script.m" file.
    ### You may change it to launch your script.
    ### 
    ### Change R2018a with your desired version

    /software/MATLAB/R2018a/bin/matlab -nodesktop -nosplash -r "script"
    # /software/MATLAB/R2018a/bin/matlab -nodesktop -nosplash -r "script" >& myMatlabOutput
    
    ### You may run also a second script
    ### or another software in the same job.
    
    /software/MATLAB/R2018a/bin/matlab -nodesktop -nosplash -r "script2"
    
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
    [<username>@<node> myScriptDir]$ /software/MATLAB/R2018a/bin/matlab -nodesktop -nosplash -r "script"
    [<username>@<node> myScriptDir]$ /software/MATLAB/R2018a/bin/matlab -nodesktop -nosplash -r "script" >& myMatlabOutput &

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
    [<username>@<node> myFolder]$ /software/MATLAB/R2018a/bin/matlab -nodesktop -nosplash -r "script"``


.. _MatlabMultipleNodes:

Interactive job on multiple nodes
-------------------------------

This guide is intended to guide the user through the submission of a Matlab job on multiple nodes.

Up to now, it was possible to launch a Matlab job on a single node (using all or a part of the available cores on the nodes) using the ‘local’ profile to start a Matlab parpool.
However, the job failed if it was asked to run on multiple nodes.

The solution to this problem is to create a new Matlab Cluster configuration.

To do so, it is necessary to: 

1.	Go in the Home tab, click on Parallel -> Create and Manage Clusters (under Environment);

.. figure:: images/MatlabParallel1.png

2.	In the Cluster Profile Manager, click on Add Cluster Profile -> Generic;

.. figure:: images/MatlabParallel2.png

3.	In the Cluster Profile Manager, select the newly created Generic Profile and select Edit (down on the right of the Cluster Profile Manager windows);

4.	Now you can insert a description of the Cluster, specify the NumWorkers property by setting it to 320 (max number of workers that can be required by the user) and fill the Plugin Scripts Location query. Select browse to specify the folder where you have the matlab plugin script for sun grid engine (i.e.: ‘/global-scratch/bulk-pool/USER_XX/matlab-parallel-gridengine-plugin’). Fill the Additional Properties query by indicating as Value for the Name “Queue” the corresponding queue where you want to submit the job (i.e.: mecc4.q, mecc2.q, cfdguest.q, all.q, hub.q, etc…). By default, if no queue is indicated, the job will run on “mecc4.q”. Click Done (down on the right of the Cluster Profile Manager window);

**T.B.N.: The ad-hoc version of the matlab-parallel-gridengine-plugin folder has to be requested to the system administrator of your section (the version that you can find online had to be changed to ensure compatibility with the CFDHUB cluster).**

.. figure:: images/MatlabParallel3.png

5.	Now it is possible to perform the validation of the newly created Generic Profile. Select the Validation tab in the Cluster Profile Manager, deselect SPMD job test, choose the number of workers to use for the validation (of course, select a number of workers-cores big enough so as to have two or more nodes working) and click on Validate to test the profile (down on the right of the Cluster Profile Manager window). If one of the tests failed, please contact the system administrator.

.. figure:: images/MatlabParallel4.png

If everything went fine, you are now able to launch Matlab jobs on multiple nodes.

The whole procedure could be performed both using the Matlab GUI or working directly from the matlab terminal (programmatically, please refer to the mathworks site if you chose to use this option).

Of course, when starting the parpool it will be mandatory to specify that now we want to use the newly created Generic Profile, followed by the number of workers that we want to use.

i.e.: 	| ``parpool(‘GenericProfile’,80)``
i.e.:	
::

    p = gcp('nocreate');
    if isempty(p)
        myPC = parcluster('Gen1');
        myPC.AdditionalProperties.Queue = 'mecc4.q';
        parpool(myPC,80);
    end
        
Moreover, if the script/function that we intend to run in parallel on multiple nodes relies on other scripts/functions created by the user, it is necessary to specify such files and paths in the “Attached Files” and “Additional Paths” quey in the Cluster Profile Manager (of course, the same could be done programmatically).


.. _OpenFOAM:

-------------------------
OpenFOAM
-------------------------

OpenFOAM is available on CFDHub.

All three OpenFOAM versions are available:

- `OpenFOAM by ESI <https://www.openfoam.com>`_;
- `FOAM-Extend Project <https://foam-extend.sourceforge.io>`_;
- `OpenFOAM-Foundation <https://openfoam.org>`_;

The software is available through modules. 
To make the system aware of OpenFOAM modules using the relevant module (``module use ...``), looking for the relevant version (``module avail``) and loading the choosen version (``module load ...``):

::

    module use /software/modulefiles/CFD
    module avail
    module load openfoam-v2106

Sometimes you may find an issue to run OpenFOAM in parallel (*bash: mpirun: command not found...*). To solve the problem, yo need to load openmpi using:

::

    module use /software/modulefiles/parallel/
    module load openmpi-4.1.3-gcc.7.3.0

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
    #$ -l h_rt=3:00:00      # jobDuration hh:mm:ss
    #$ -q hub.q             # queueName
    #$ -pe mpi 4            # cpuNumber
    #---------------------------------------------------------
    
    ### LOAD THE OPENFOAM ENVIRONMENT
    module use /software/modulefiles/CFD
    module load openfoam-v2106
    
    # module use /software/modulefiles/parallel/     # if necessary
    # module load openmpi-4.1.3-gcc.7.3.0            # if necessary
    
    #---------------------------------------------------------
    
    ### EXECUTE COMMANDS
    #./Allrun
    
    blockMesh >& log.blockMesh
    decomposePar >& log.decomposePar
    mpirun --hostfile machinefile.$JOB_ID snappyHexMesh -parallel >& log.snappyHexMesh
    mpirun --hostfile machinefile.$JOB_ID simpleFoam -parallel >& log.simpleFoam
    reconstructPar -latestTime >& log.reconstructPar
    sample -latestTime >& log.sample

    echo End Parallel Run

Just add/remove *hashtags* [#] to comment/uncomment the lines. To execute the commands, you may either include an executable file (``Allrun`` in this case), or list all relevant commands.

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
    [<username>@<node> ~]$ module use /software/modulefiles/CFD
    [<username>@<node> ~]$ module load openfoam-v8
    # [<username>@<node> ~]$ module use /software/modulefiles/parallel/     # if necessary
    # [<username>@<node> ~]$ module load openmpi-4.1.3-gcc.7.3.0            # if necessary
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
    [<username>@<node> ~]$ module use /software/modulefiles/CFD
    [<username>@<node> ~]$ module load openfoam-v8
    # [<username>@<node> ~]$ module use /software/modulefiles/parallel/     # if necessary
    # [<username>@<node> ~]$ module load openmpi-4.1.3-gcc.7.3.0            # if necessary
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
    #$ -l h_rt=2:00:00      # jobDuration hh:mm:ss
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


.. _SpackSection:

-------------------------
Spack
-------------------------

