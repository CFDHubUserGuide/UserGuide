===================
HOW TO SUBMIT A JOB
===================

To perform a calculation on CFDHub there are some information that need to be addressed.

- :ref:`Access to CFDHub<SSHConnection>`
- :ref:`Data Transfer on the cluster<DataTransferring>`
- :ref:`Follow the workflow of a hpc facility<Workflow>`
- :ref:`Launch your job<LaunchYourJob>`
- :ref:`Software availability needs to be verified and eventually installed <Software>`

**Generally the calculation on a HPC infrastructure is performed in batch mode, without the graphical interface, since the calculation runs faster.**

When the calculation is completed, it is possible to postprocess the results using the graphical interface, or transferring the relevant files on your local machine.


.. _Workflow:

-------------------------
WORKFLOW ON HPC FACILITY
-------------------------

The system is divided in different working areas in order to permit a more sustainable and efficient use of the available resources. The main working areas are reported and described here, please check how to efficiently use each work area in order to avoid affecting the whole system functionality.

All working areas are accessible from the login nodes and from the computational nodes.

Generally source and compiled code files, libraries, documents are placed in the user ``/home``.

Files that are used to run calculations are located in the scratch area ``/fast-scratch`` and ``/big-scratch`` where the disk has a quick IO speed. The disk space is large, but is shared among all users, when the disk is full nobody will be able to work on the cluster anymore.

When a computation is finished, results should be moved to ``/ARCHIVIO``, where each Research Group has its own archive space.

A diagram of the disk architecture is shown in the picture below.

.. image:: images/cfdhubWorkAreas.png

Let's go in detail across the disks

- | ``/home``
  | *Purpose*: to save personal data such as libraries, sources, compiled code, documents etc. In general, this area is reserved to files that you think should be backed up.  
  | *Capacity*: a quota for each group is assigned, this limitation permits to avoid the filling up of the ``/home`` area affecting other groups or users. 
  | To know the total quota and the actual occupancy of the available space type ``repquota –augs``  
  | *Access*: all nodes  
  | *Backup*: YES 

- | ``/fast-scratch`` & ``/big-scratch``
  | *Purpose*: launch runs and put data actually on use.
  | In order to preserve the purpose of this area and avoid a filling up of the area all data older than 60 days will be deleted from this area.
  | Please be careful and move your data to ``/ARCHIVIO`` area when they are not on use anymore
  | *Capacity*: approx. 6Tb to 30Tb on SSD (high speed) cache disk interfaces (normal) NLSAS disks to speed up data exchange processes.
  | *Access*: all nodes
  | *Backup*: NO  

- | ``/ARCHIVIO``
  | *Purpose*: save the results and data you want to keep for long term.
  | This area permits to store data without affecting the running processes in other working areas.
  | *Capacity*: related to the amount of storage purchased as a group, divided into blocks of 8Tb.
  | *Access*: all nodes
  | *Backup*: NO, however considered reliable being residing on enterprise band hard-drives with multi-disk data redundancy


.. _LaunchYourJob:

-------------------------
LAUNCH YOUR JOB
-------------------------

**A job can be launched only on a computational node.**

To launch you job, three possibilities are available:

- :ref:`Batch job using a queue<BatchJob>`;

- :ref:`Interactive job using a queue<InteractiveQueue>`;

- :ref:`Interactve job working on a node<InteractiveNode>`;.

The submission of jobs through a queue require some instructions to the job scheduler to reserve a node (or some cpus of a node) to the user. The Job Scheduler of CFDHub is `PBS Pro`__. In the :ref:`Software <Software>` section you may find some job submission examples.

.. _PBS: https://www.altair.com/pbs-professional/

__ PBS_

Batch jobs require a script with the instructions. In the :ref:`Software<Software>` section you may find some script examples for your specific application.

Interactive jobs require the user to give interactively the instructions to the computational node using single commands or using a script.


.. _BatchJob:

_________________________
Batch jobs using a queue
_________________________

To submit a job through the Job Scheduler you need to prepare a launch file, to specify the computational requests. In this case, you need to ask to the job scheduler the necessary resources. The job scheduler will assign to you the resources as soon as they are available.

A launch file ``launch.sh`` is a shell script that has the following instructions:

::

       #!/bin.bash             # use bash as command interpreter
       #$ -cwd                 # currentWorkingDirectory
       #$ -N jobName           # jobName
       #$ -j y                 # merges output and errors
       #$ -S /bin/bash         # scripting language
       #$ -l walltime=1:00:00  # jobDuration hh:mm:ss
       #$ -q hub.q             # queueName
       #$ -pe mpi 16           # cpuNumber
      
       ### Specify the executable...
       ./an_executable
       
       echo End Parallel Run

This script will launch a job in ``-cwd`` (the current workinf directory), the name of the job is ``jobName`` (for monitoring purposes), its durations will be 1 hour (``walltime=1:00:00``), the queue on which it will be run will be ``hub.q`` and it requires 16 cpus (``mpi 16``).

Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``queues`` you have access to.

To submit the job from the ``login node`` you need to place the launch file ``launch.sh`` in the job folder, and submit it:

``[user@nodevg-0-1 currentWorkingDirectory]$ qsub launch.sh``

Useful commands are:

+-----------+---------------------------------+-----------------------------+
| Command   | Description                     | Example                     |
+===========+=================================+=============================+
| ``qsub``  | Submit a job                    | ``qsub launchFile.sh``      |
+-----------+---------------------------------+-----------------------------+
| ``qstat`` | Show status of jobs             | ``qstat -u <username>``     |
+           +---------------------------------+-----------------------------+
|           | Show status of queue            |``qstat -f -q all.q``        |
+-----------+---------------------------------+-----------------------------+
| ``qdel``  | Delete a job                    | ``qdel 84249``              |
+-----------+---------------------------------+-----------------------------+
| ``qmove`` | Move a job to a different queue | ``qmove hub.72 84249``      |
+-----------+---------------------------------+-----------------------------+

``-pe mpi ##`` indicates the type of computational unit to be allocated, while ## is the number of processors to allocate.Each unit can be allocated in any node listed in the specified queue (-q xxx.q): the job scheduler will decide the nodes in which allocate the resources. 

Bigger cpu clusters can be defined: –pe mpi_20 means that a unit of 20 CPUs will be allocated. Each computational unit (of for example 20 CPUs) is stored entirely in a single node and not divided in multiple ones (the unit dimension cannot exceed the number of CPUs physically present in a node). If the computational unit is bigger than 1, ## cannot be a generic number, but it must be a multiple of xx of the defined unit mpi_xx. mpi_xx is defined by the Administrator and it depends on the queue selected by the user (-q).

A list of the mpi_xx division defined for a single queue can be shown using the command:

``[username@loginNode ~]$ qconf -sq hub.q | grep pe_``

.. while for the whole system the command is:
..
.. ``[username@loginNode ~]$ qconf -sql hub.q``

Some examples:  

- ``-pe mpi 20`` means that 20 single CPU units will be allocate, distributed in all the available nodes.  

- ``-pe mpi_20 20`` means that a single unit of 20 CPUs will be considered and 20 CPUs will be allocated in a single node.  

- ``-pe mpi_20 40`` means that two units of 20 CPUs will reserved in two different nodes and 20 CPUs (per node) will be allocated in those nodes.  

- ``-pe mpi_20 60`` means that three units of 20 CPUs will reserved in three different nodes and 20 CPUs (per node) will be allocated in those nodes. 



.. _InteractiveQueue:

______________________________
Interactive job using a queue
______________________________

Jobs may be launched on a queue interactively. This job submission can be useful to check that the computation you are preparing will run without errors in batch mode.
In this case, you need to ask to the job scheduler the necessary resources. The job scheduler will assign to you the resources as soon as they are available.

To request the interactive resources to the job scheduler:

``qrsh -q hub.q -l h_rt=2:00:00 -l h_vmem=2G -pe mpi 2``

These instructions request on the *hub.q* queue (``-q hub.q``) 2 cpus (``-pe mpi 2``) for 2 hours (``-l h_rt=2:00:00``, hh:mm:ss) and 2GB of RAM (``-l h_vmem=2G``).
You can adjust the request according to your need.

Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``queues`` you have access to.

Yuo may prepare an *alias* for this command to be written in the *.bash_aliases* file in your *home* directory:

``alias interactive_hub_02h_2cpu='qrsh -q hub.q -l h_rt=2:00:00 -l h_vmem=2G -pe mpi 2'``

so when you need to ask for 2 cpus in interactive mode for 2 hours you just type:

``[username@nodevg-0-1 currentWorkingDirectory]$ interactive_hub_02h_2cpu``



.. _InteractiveNode:

_________________________________
Interactive job working on a node
_________________________________

| **You will always have to log into a computing node before running any program.**
| Running a program by mistake on the “master” or “login node” will slow down, if not block, every user connection… So be careful!

Please check with your :ref:`CFDHub Contact Person<ContactPerson>` what are the ``nodes`` you have access to.

After having logged to the cluster through the VNC client, you will have the possibility to open a terminal.

You can briefly look at node usage opening on the cluster side Firefox or any web browser and looking at the web page http://master/ganglia.

From there you can have an overview of the CPU usage of all the nodes. The main drawback is that it still does not allow you to check if a node is *completely* free, i.e., someone may still be using the node to set-up or post-process a case… You will have to follow the next instructions to check completely the availability of a computing node.

1. | Log into the node running this command on the terminal:
   | ``[<username>@nodevg-0-X ~]$ ssh -Y <node>``

2. | After having accessed the node you must check the active processes of all users: 
   | ``[<username>@<node> ~]$ ps -aux``
   | It will list all the processes and you will have to look for the ones that may use CPU (3rd column) and/or RAM memory (4th column). You should not occupy a computing node if you see processes that use CPU and/or RAM. It means it is not free. If you have doubts you can ask your :ref:`CFDHub Contact Person<ContactPerson>`.


