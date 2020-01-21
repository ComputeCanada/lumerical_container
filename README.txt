This singularity recipe creates a container for running the lumerical GUI, fdtd-solutions.

A version of lumerical can be downloaded here: https://www.lumerical.com/downloads/customer/
after creating a free account. A license will be required to run the software. This recipe uses the 2019b release for Linux.

To create the singularity container run the following commands after singularity has been installed on a Linux machine where you have root permissions. In this particular case it was a Ubuntu 18 VM.

 $ sudo singularity build lumerical_GUI.simg singularity_recipe_centos

Once the container is built, copy it to a Compute Canada cluster. Start up an interactive job, and run an instance of the container e.g.

 $ salloc --time=1:0:0 --mem=4G --ntasks=1 --account=def-someuser
 $ module load singularity
 $ singularity instance start lumerical_GUI.simg vnc1

You now have a vncserver running in an instance of the container, named vnc1. Then to connect to it setup an SSH tunnel through the cluster head node to the node where the interctive job and the vncserver are running. Once the tunnel is setup you can connect to the vncserver through the tunnel.

Once connected to the vncserver, you should see a very minimal MATE desktop. To start a terminal right click on the desktop and select "Open in terminal". Then to run lumerical GUI set the environment variable describing how to connect to the license server, and start the lumerical GUI with

 $ fdtd-solutions

To stop the vnc1 container instance run

 $ singularity instance stop vnc1

in the interactive job shell.
