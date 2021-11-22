Assignment 1 CMPE 283
========================================
Team Memebers:

Venkata Prithvi Raj Namburi (015944994)

Hema Aishwarya Talluri (015948816)

==========================================

Work Implemented by each member
==========================================
Prithvi Raj: 
- To create a local copy of Linux Kernel, downloaded and built the Linux modules and required libraries.
- Changed the cmpe283-1.c code and added custom logic
- After inserting the module and printing from the Kernel, committed the cmpe283-1.c code file and Makefile.

Aishwarya:
- Verified the code functionality with sample ouputs if it's working or not
- Generated files and made commits to GIT


Steps Used To Build And Complete This Assignment 1:
==========================================

PreRequirement: Use a virtualization enabled machine with atleast 100 GB or more space.

Step 1: First of all, Get VmWare Fusion setup installed.

Step 2: Then install Linux Ubuntu on VMware Fusion. 

Step 3: Then install Git

Step 4: Then fork the Linux Repo in your GitHub Account.

Step 5: Then pull the Linux Git Repo into your Ubuntu Linux.

Step 6: make menuconfig

Step 7: make oldconfig (press enter to answer all the questions)

Step 8: make prepare

Step 9: make -j 4 (Number indicates the number of cores alloctaed)

Step 10: "sudo make INSTALL_MOD_STRIP=1 modules_install"

Step 11: "sudo make install" (Installs the modules)

Step 12: "sudo reboot" (It should reflect the changed kernel)

Step 13: "uname -a" (To check the new compiled module)

Step 14: Now run the "make" command to check for any errors. There should be an error pointing missing license. 
			Open the cmpe283-1.c file and add (MODULE_LICENSE("GPL v2");) at the end of the file and save the changes

Step 15: Now run the make command again
			After it is successfully executed, check that the .ko file is created using "lsmod | grep cmpe283"

Step 16: We insert our module by running the command "sudo insmod cmpe283-1.ko"

Step 17: To check if it is successfully inserted, run "dmesg"

Step 18: To remove the module run "sudo rmmmod cmpe283-1"

Step 19: To check if it is exited, run "dmesg"

Step 20: After successfully completing all the steps, Commit the changes and push to GIT.

 
