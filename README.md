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
    "sudo rmmmod cmpe283-1"
   - run "dmesg" to check the exit message ""CMPE 283 Assignment 1 Module Exits"

 
