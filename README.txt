This singularity recipe creates a container for running the lumerical GUI, fdtd-solutions.

To create the singularity container the following steps should be performed on a Linux computer where singularity has been installed on which you have root permissions. In this particular case it was a Ubuntu 18 VM.

A version of lumerical can be downloaded here: https://www.lumerical.com/downloads/customer/
after creating a free account. A license will still be required to run the software. This recipe uses the 2019b release for Linux as it is compatible with older versions of the license server software. To access the exectuables an rpm package inside the downloaded tar.gz file must be extracted with the following commands:

$ tar -xzf Lumerical<version>.tar.gz
$ cd Lumerical<version>/rmp_install_files
$ rmp2cpio < Lumerical<version>.rpm | cpio -i -d

Modifications are likely required to the recipe in the "%files" section to reflect the location of the downloaded and unpacked version of lumerical.

The singularity container can then be created from the recipe with the following command.

 $ sudo singularity build lumerical_GUI.simg singularity_recipe_centos

This will take some time as it must download and install software needed for running the GUI desktop MATE which is quite large.

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
