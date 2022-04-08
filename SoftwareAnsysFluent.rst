.. COPIARE IN SEGUITO IN SOFTWARE
=====================
ACCESS TO THE CLUSTER 
=====================

.. _AnsysFluent:

-------------------------
Ansys Fluent
-------------------------

This section presents how to start and configure Ansys Fluent on the cluster. The user is expected to have some experience with this CFD code since this user guide covers how to access Ansys Fluent on a single or multiple nodes. 

.. _LAUNCH_FLUENT_FROM_TERMINAL:
__________________________________
LAUNCH FLUENT FROM TERMINAL
__________________________________

Before launching Ansys Fluent is necessary to enter in the node where the user want to run the simulation. 
.. ( nella vecchia guida aggiungeva un “ , as explained in section 4.1 and 4.2”; rimetterlo? Se si, aggiornare a che capitoli a cui fa riferimento)
After the user has logged from the terminal to the node, should go into the folder where the case to be run is located. From this folder the user should launch the command:
    ``/software/ansys_inc/v212/fluent/bin/fluent``
    .. ``/fluent`` 
.. ( se si userà l’approccio dei moduli stile cineca, aggiornare mettendo il solo comando e non il path del comando, verificare che sia questo il comando ) 
.. _PARALLEL_SINGLE_NODE:
__________________________________
PARALLEL AND SINGLE NODE SET-UP 
__________________________________

.. figure:: images_test/single_node.png

.. _PARALLEL_MULTIPLE_NODE:
__________________________________
PARALLEL AND MULTIPLE NODE SET-UP
__________________________________

.. figure:: images_test/multiple_node.png

.. _CPU_GPU:
__________________________________
PARALLEL CPU+GPU SET-UP
__________________________________

.. _CLEAN_CRASHED_PROCESSES:
__________________________________
HOW TO MANAGE A CRUSHED PROCES
__________________________________
