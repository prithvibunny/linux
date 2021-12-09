Assignment 4
========================================
Team Memebers:

Venkata Prithvi Raj Namburi (015944994)

Hema Aishwarya Talluri (015948816)

==========================================

Work Implemented by each member
==========================================
Prithvi Raj: 
- Make changes to configure shadow paging instead of nested paging
- Note the exit counts for shadow paging and record observations

Aishwarya:
- Note the exit counts with assignment 3 configuration and record observations


Steps Used To Build And Complete This Assignment 4:
==========================================

Start with the Assignment-3 setup

Run dmesg command to see the current count of exits for all the exit reasons

With EPT :

![4](https://user-images.githubusercontent.com/58871002/145307095-54a93201-eb59-4392-ba9b-841a7ec9e353.png)

After insterting the module with ept =0 :

![4 1](https://user-images.githubusercontent.com/58871002/145307003-bff69091-1287-4124-b132-b4837721b99d.png)

Answers to questions based on our obeservations:

The output of exits is not what we expected after observing the exits in nested paging. For some of the exits which were very low or not present in case of nested paging, we see a lot of exits with shadow paging.

Contrary to what we see in nested paging , in shadow paging we do not see EPT exits, rather we see most exits in case of CR_ACCESS. We also see exits for INVLPG , INVPCID. We see a lot of exits in case of CR_ACCESS because now the hypervisor takes exit when control register is accessed and during INVLPG because hypervisor takes exit to invalidate translations in TLB.

===================================================================

Assignment 3 
========================================
Team Memebers:

Venkata Prithvi Raj Namburi (015944994)

Hema Aishwarya Talluri (015948816)

==========================================

Work Implemented by each member
==========================================
Prithvi Raj: 
- Modify the vmx.c file based on the findings
- Test the modifications made for leafnodes using cpuid package in the inner VM
Aishwarya:
- Modify the cpuid.c file
- Test the modifications made for leafnodes using cpuid package in the inner VM


Steps Used To Build And Complete This Assignment 3:
==========================================

Start with the Assignment-2 setup

Modify the cpuid.c file and vmx.c file as follows

When the leafnode eax value is 0x4FFFFFFE,Return the high 32 bits of the total time spent processing all exits in %ebx and the low 32 bits of the total time spent processing all exits in %ecx. We achieve this by declaring a variable to store the total time and adding the time spent processing each exit(calculated by reading timestamp before and after the exit) to this total time.

When the leafnode eax value is 0x4FFFFFFC and there is exit number provided in ecx as input, Return the time spent processing that particular exit Return the high 32 bits of the total time spent for that exit in %ebx and the low 32 bits of the total time spent for that exit in %ecx. We adhieve this by declaring an array for all the exits and storing time spent for each exit in the corresponding array element.

save the modifications in both the files

Run the following sequence of commands

make -j 4 modules

make INSTALL_MOD_STRIP=1 modules_install && make install

run "lsmod | grep kvm" to see if the module is already loaded

if the comamnd returns that the module is already loaded run "rmmod kvm" and "rmmod kvm_intel" if both are present

run "modprobe kvm"

Now in order to test the modifications we made we can open the nested VM we have installed in the assignment-2 steps

Now In the terminal we can use the commands "cpuid -l 0x4FFFFFFE" to see the total time spent processing all exits in ebx and ecx.

We can use the command "cpuid -l 0x4FFFFFFC -s {exit_reason}" to see the total time spent processing the exit provided in {exit_reason}.

![3](https://user-images.githubusercontent.com/58871002/145306822-7140df36-ebcd-4cc5-8479-fd4e95ebdf2a.png)

Questions for 0x4fffffffc :

The increase in the number of total exits is not stable. We notice a very high number of exits for MSR access.For a full vm reboot we noted around 1174554 exits.

The most frequent exits are EPT violations and MSR access and the least frequent exits are 0 (for VMWRITE, triple fault, etc) and there are few exits for DR_Access,APIC access.


===================================================================



Assignment 2 
========================================
Team Memebers:

Venkata Prithvi Raj Namburi (015944994)

Hema Aishwarya Talluri (015948816)

==========================================

Work Implemented by each member
==========================================
Prithvi Raj: 
- Modify the cpuid.c file
- Install virtual machine manager on the VM
- Install ubuntu on a inner VM using the virtiual machine manager
- Install the cpuid package on the innervm
- Test the modifications made for leafnodes using cpuid package in the inner VM
Aishwarya:
- Modify the vmx.c file based on the findings
- Run the commands required to build the module after the modifications
- Test the modifications made for leafnodes using cpuid package in the inner VM

Steps Used To Build And Complete This Assignment 2:
==========================================

Start with the Assignment-1 setup

Modify the cpuid.c file and vmx.c file as follows

Return the total no. of exits in eax for leafnode for eax values as 0x4fffffff

Retrun the number of exits in eax for the exit type provided in ecx when the leaf node value is 0x4ffffffd

We can find the modified values for cpuid.c under arch/x86/kvm/cpuid.c and for vmx.c under arch/x86/kvm/vmx/vmx.c

We declare 2 extern variables in vmx.c one for total exits and an array to store total exits for each reason

We increment the total exits each time there is an exit and we increment the corresponding value for that reason's index in the array

save the modifications in both the files

Run the following sequence of commands

make -j 4 modules

make INSTALL_MOD_STRIP=1 modules_install && make install

run "lsmod | grep kvm" to see if the module is already loaded

if the comamnd returns that the module is already loaded run "rmmod kvm" and "rmmod kvm_intel" if both are present

run "modprobe kvm"

Now in order to test the modifications we made to kvm we need to install a VM inside our VM

From the ubuntu software center install virtual machine manager

Download the ubuntu iso file and install a ubuntu VM following the same steps followed in Assignment-1

Once installed, in the terminal of our inner VM install the cpuid package using the command "sudo apt-get install cpuid"

Now we can use the commands "cpuid -l 0x4fffffff" to see the total exits returned in eax

![2](https://user-images.githubusercontent.com/58871002/145305563-53a1d7fe-dfa6-4279-b929-990a370d0b0d.png)

We can use the command "cpuid -l 0x4fffffffd -s {exit_reason}" where exit reason is an integer for the exit reason for which we want to see the number of exits We can use it for example as "cpuid -l 0x4ffffffd -s 31" to see the number of exits due to RDMSR .

![2 1](https://user-images.githubusercontent.com/58871002/145306374-1a1b5891-bd02-4c3a-9045-333d98906d2a.png)

Questions for 0x4fffffffd :

The increase in the number of total exits is not stable. We sometimes see an increase of 422 exits and the other time we see an increase of 677 or 700. We notice a lot of exits for EPT violations , MSR access , IO Instructions.For a full vm reboot we noted around 1174554 exits.

The most frequent exits are EPT violations and MSR access and the least frequent exits are 0 (for VMWRITE, triple fault, etc) and there are very few exits for DR_Access,APIC access.
 

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

Step 1: First of all, Get VmWare Workstation setup installed.

Step 2: Then install Linux Ubuntu on VMware Workstation. 

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
