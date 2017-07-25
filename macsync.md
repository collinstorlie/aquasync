### Synchronise Files Between Your Mac & The HPC

'rsync' is a program designed to synchronise files between two computers, it comes pre-installed on your Mac Computer and is accessed via the Terminal.
We will now setup 'rsync' to synchronise files between a chosen directory on your computer, and a directory in your HPC home account.
The directory we synchronise to in your HPC home account will be a 'symlink' to your project space within the Aquaculture Group shared HPC storage.
This means the files you synchronise will be viewable by other members of your Aquaculture Project Group.

1. To start, ensure you are logged into the HPC and in your HOME directory
```
ssh hpc
cd $HOME
```       
2. The shortcut (symlink) to your Aquaculture Project space should already exist, you can confirm this by listing the files in your home directory 
```        
ls -laF
```        
* Here is an example of what a symlink looks like when listed, please note the paths of your symlink will be different:
<img src="img/symlink.png">
* You should be presented with a long listing of files / directories within your HPC home.  
* Look for a line where the permissions begin with 'l' and where the name is followed by a '-->' pointing to another directory location.
* Your symlink should point to '/home5/aqua/' followed by your Aquaculture Project Groupname (e.g. barra, edna, prawn, or gen).  
* For example, if you're in the eDNA project group, you should have a symlink which points to '/home5/aqua/edna/'.  Make a note of your symlink name, we will need it for next steps.
3. Now that you have the name of your symlink directory, we can proceed to configure your rsync script. Start by disconnecting from the hpc, use the 'exit' command
```         
exit
```     
4. Decide which local folder you'd like to synchronise to the HPC (can be anywhere on your computer)
5. For example, if you wanted to synchronise a directory called jcXXYYYY\_AQSYNC on your Desktop, you could first create the folder
```        
mkdir ~/Desktop/jcXXYYYY_AQSYNC
```
6. Then navigate to it: 
```        
cd ~/Desktop/jcXXYYYY_AQSYNC/
```        
7. Create your rsync shell script using 'touch'
```        
touch rsync.sh
```      
8. Use 'nano' to open and edit the file
```        
nano rsync.sh
```
9. Type the following lines into your shell script exactly (substituting your jc numberr and the name of your SYMLINK directory in the second argument):
```
#!/bin/bash
rsync -avz ~/Desktop/jcXXYYYY_AQSYNC HPC:/homeN/XX/jcXXYYYY/MYSYMLINK
```  
* In the above command, your directory path will start with 'homeN', where 'N' is a number between 1 and 6. If you can't remember which 'home' your HPC count resides in.  
Simply log-in to the HPC with the command `ssh HPC` then run the command `pwd` to display the full directory path of your HPC home.       
10. To exit nano and save this file:
* Use the key binding CTRL+X
* Then type 'y' to confirm the file name and press ENTER again

### Automate File Synchronisation Between Your Mac & The HPC