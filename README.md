##   CMPE 283

### Assignment-1


#### Team Members
- Pratiksha Shukla
- Abhishek Yadav

#### Team Members contribution

Pratiksha Shukla:
- Downloaded and configured VMWare workstation
- Set up ubuntu OS using ubuntu iso file and turn on virtualizing capabilities 
- Install and configure git and clone the created repo 
- Download and add the .c and the makefile from canvas and add to our cloned repo under the cmpe283 folder
- configure the VM to be able to run the make commands by installing necessary tools
   run certain install commands 
   like libncurses-dev flex bison openssl etc
- Document the steps followed to be included in the ReadMe file

Abhishek Yadav
- Fork the original torvalds/linux repository to personal github
- Edit the .c file to add the capability info for the primary procbased controls, secondary procbased controls,
 entry and exit controls after referring to the SDM volume 3 module 23.6.x and 23.7 and 23.8
- Run make menuconfig and  copy current config to .config
- Edit the .config file to disable errors due to keys 
- Run sequence of make commands to compile the linux source code
- Create our kernel module and insert into the kernel
- Push the created modules to the forked repo
- Check the capability info and remove kernel module 



#### Steps involved in completing the assignment

- Download and install VMware workstation
- Download ubuntu iso file
- Create a ubuntu virtual machine on vmware workstation.
- Setup Ubuntu selecting 200 GB of space and 5gb of memory
- Go into settings of the virtual machine and in the processor section , 
 check the checkboxes for virtualization capabilities
- Fork the original linux repository from torvalds/linux into your github
- In the ubuntu virtual machine terminal run the command cat /proc/cpuinfo to check that 
  vmx flags are present and virtualization capabilities are present for the VM.
- Install git on the VM by using the command "sudo apt-get-install git"
- Clone the repository which we forked earlier from torvalds/linux
- Run cd linux to siwtch to the repo which we just cloned
- In the cmpe283-1.c file downloaded from canvas add the sections of code for primary procbased controls, secondary procbased controls,
 entry and exit controls after referring to the SDM volume 3 module 23.6.x and 23.7 and 23.8.
- Add the modified cmpe283-1.c file and the Makefile to a folder cmpe283 and put the folder under the cloned linux directory
- switch to the linux directory
- Run "uname -a" command to check the  version (check if there is a version mismatch between the cloned 
  version and the current vm linux distribution we are running)
- In order to run certain make commmands we need to run certain install commands 
  "sudo apt-get install libncurses-dev flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf"
- Follow the following sequence of commands to build and install the kernel modules
 - "make menuconfig" (do not save any changes in the UI and exit it  )
 - "cp /boot/config-{curent version we got from "uname -a" command without braces} .config"
 - "make oldconfig"   (press enter to answer all the questions)
 - "make prepare" 
 -  "make -j {no. of cores on the processor} modules"   eg : "make -j 4 modules"
 -  If there is an error similar to"*** No rule to make target 'debian/canonical-certs.pem'"
 - vi the .config file and set the system trusted keys and revocation keys strings =  "" 
 - after the "make -j 4 modules" command is sucessfuly finished
 - "make -j 4"
 - After this command is succesfully finished we will see a "is ready" statement in the end
 -  "sudo make INSTALL_MOD_STRIP=1 modules_install"
 -  "sudo make install"
 -  "sudo reboot"
 -  run "uname -a" to check that the version has changed (updated to our new complied module)
 -  change directory to the drectory we created earlier "cd cmpe283" 
 -  run "make" command
    check if there are any errors in the file or an error asking to include license. In case 
    of an error for license, go to the cmpe283-1.c file and at the botton add
    "MODULE_LICENSE("GPL v2");" and save the file.
 - After the make command is successfuly executed run , check that the .ko file is created using
   "lsmod | grep cmpe283" 
  -  "sudo insmod cmpe283-1.ko" to insert our module
  - Run "dmesg" command to check
  - We can see the capability info for all the MSR which we have defined in the cmpe283-1.c file undr the heading
    "CMPE 283 Assignment 1 Module Start"
  - To remove the module run 
    "sudo rmmod cmpe283-1"
   - run "dmesg" to check the exit message ""CMPE 283 Assignment 1 Module Exits"


-------------------------------------------------------------------------------------------------------------------------------------

#### ASSIGNMENT 2


#### Team Members contribution

Pratiksha Shukla:
- Modify the vmx.c file 
- install virtual machine manager on the VM
- install ubuntu on a inner VM using the virtiual machine manager
- install the cpuid package on the innervm
- Test the modifications made for leafnodes using cpuid package in the inner VM

Abhishek Yadav
- Find the missing and disabled exits by referring to the sdm. 
- Modify the cpuid.c file based on the findings
- run the commands required to build the module after the modifications
- Test the modifications made for leafnodes using cpuid package in the inner VM


#### Steps to complete

- Start with the Assignment-1 setup
- Modify the cpuid.c file and vmx.c file  as follows
- Return the total no. of exits in eax for leafnode for eax values as 0x4fffffff 
- Retrun the number of exits in eax for the exit type provided in ecx when the leaf node value is 0x4ffffffd
- We can find the modified values for cpuid.c under arch/x86/kvm/cpuid.c and for vmx.c under arch/x86/kvm/vmx/vmx.c
- We declare 2 extern variables in vmx.c one for total exits and an array to store total exits for each reason
- We increment the total exits each time there is an exit and we increment the corresponding value for that reason's index in the array
- save the modifications in both the files
- Run the following sequence of commands
- make -j 4 modules
- make INSTALL_MOD_STRIP=1 modules_install && make install
- run "lsmod | grep kvm" to see if the module is already loaded
- if the comamnd returns that the module is already loaded run "rmmod kvm" and "rmmod kvm_intel" if both are present
- run "modprobe kvm"
- Now in order to test the modifications we made to kvm we need to install a VM inside our VM
- From the ubuntu software center install virtual machine manager
- Download the ubuntu iso file and install a ubuntu VM following the same steps followed in Assignment-1
- Once installed, in the terminal of our inner VM install the cpuid package using the command "sudo apt-get install cpuid"
- Now we can use the commands "cpuid -l 0x4fffffff" to see the total exits returned in eax 
- ![Screenshot (193)](https://user-images.githubusercontent.com/89236413/142789745-6a88c451-da39-4971-88af-5ec4e861238f.png)

- We can use the command "cpuid -l 0x4fffffffd -s {exit_reason}" where exit reason is an integer for the exit reason for which we want to see the number of exits
  We can use it for example as "cpuid -l 0x4ffffffd -s 31" to see the number of exits due to RDMSR .
![Screenshot (191)](https://user-images.githubusercontent.com/89236413/142789612-91f143fa-e304-4687-8aba-fa88f6408cf6.png)

- Questions for 0x4fffffffd : 
 - The increase in the number of total exits is not stable. We sometimes see an increase of 422 exits and the other time we see an increase of 677 or 700. We notice a lot of        exits for EPT violations , MSR access , IO Instructions.For a full vm reboot we noted around 1174554 exits. 
 - The most frequent exits are EPT violations and MSR access and the least frequent exits are 0 (for VMWRITE, triple fault, etc) and there are very few exits for DR_Access,APIC      access.


-------------------------------------------------------------------------------------------------------------------------------------

#### ASSIGNMENT 3

- to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested.
- Implemented in assignment :
- CPUID leaf node %eax=0x4FFFFFFE
- CPUID leaf node %eax=0x4FFFFFFC and exit reason as input in ecx

#### Team Members contribution

Pratiksha Shukla:
- Modify the vmx.c file 
- Test the modifications made for leafnodes using cpuid package in the inner VM

Abhishek Yadav
- Modify the cpuid.c file based on the findings
- Test the modifications made for leafnodes using cpuid package in the inner VM


#### Steps to complete

- Start with the Assignment-2 setup
- Modify the cpuid.c file and vmx.c file  as follows
- When the leafnode eax value is 0x4FFFFFFE,Return the high 32 bits of the total time spent processing all exits in %ebx and the low 32 bits of the total time spent processing     all exits in %ecx. We achieve this by declaring a variable to store the total time and adding the time spent processing each exit(calculated by reading timestamp before and     after the exit) to this total time.
- When the leafnode eax value is 0x4FFFFFFC and there is exit number provided in ecx as input, Return the time spent processing that particular exit  Return the high 32 bits of   the total time spent for that exit in %ebx and the low 32 bits of the total time spent for that exit in %ecx. We adhieve this by declaring an array for all the exits and         storing time spent for each exit in the corresponding array element.
- save the modifications in both the files
- Run the following sequence of commands
- make -j 4 modules
- make INSTALL_MOD_STRIP=1 modules_install && make install
- run "lsmod | grep kvm" to see if the module is already loaded
- if the comamnd returns that the module is already loaded run "rmmod kvm" and "rmmod kvm_intel" if both are present
- run "modprobe kvm"
- Now in order to test the modifications we made we can open the nested VM we have installed in the assignment-2 steps
- Now In the terminal we can use the commands "cpuid -l  0x4FFFFFFE" to see the total time spent processing all exits in ebx and ecx.
- We can use the command "cpuid -l 0x4FFFFFFC -s {exit_reason}" to see the total time spent processing the exit provided in {exit_reason}.
- ![Screenshot (195)](https://user-images.githubusercontent.com/89236413/143999757-7f0444a7-ae14-4001-b608-8aaa8e8cda66.png)

- Questions for 0x4fffffffc : 
- The increase in the number of total exits is not stable. We notice a very high number of exits for  MSR access.For a full vm reboot we noted around 1174554 exits. 
- The most frequent exits are EPT violations and MSR access and the least frequent exits are 0 (for VMWRITE, triple fault, etc) and there are few exits for DR_Access,APIC         access.



-------------------------------------------------------------------------------------------------------------------------------------

#### ASSIGNMENT 4 
Nested Paging vs. Shadow Paging


#### Team Members contribution

Pratiksha Shukla:
- Note the exit counts with assignment 3 configuration and record observations

Abhishek Yadav
- Make changes to configure shadow paging instead of nested paging
- Note the exit counts for shadow paging and record observations

#### Steps to complete

- Start with the Assignment-3 setup
- Run dmesg command to see the current count of exits for all the exit reasons
- With EPT :
![Screenshot (198)](https://user-images.githubusercontent.com/89236413/144729353-5c557296-872a-4ef7-986d-4e820e4f8927.png)

- After insterting the module with ept =0  :
![Screenshot (207)](https://user-images.githubusercontent.com/89236413/144729399-175aa5fe-47fc-4a84-9e20-e6f814aa974a.png)


- Answers to questions based on our obeservations:
- The output of exits is not what we expected after observing the exits in nested paging. For some of the exits which were very low or not present in case of nested paging, we     see a lot of exits with shadow paging.
- Contrary to what we see in nested paging , in shadow paging we do not see EPT exits, rather we see most exits in case of CR_ACCESS. We also see exits for INVLPG , INVPCID. We   see a lot of exits in case of CR_ACCESS because now the hypervisor takes exit when control register is accessed and during INVLPG because hypervisor takes exit to invalidate     translations in TLB.  


