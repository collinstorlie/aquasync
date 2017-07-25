---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
### Synchronise Files Between Your Windows PC & The HPC

'rsync' is a program designed to synchronise files between two computers, we have already installed it as a Cygwin package, and access it via the Command Prompt.
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
<img src="{{ site.baseurl | replace:'http:','https:' }}/img/symlink.png">
* You should be presented with a long listing of files / directories within your HPC home.  
* Look for a line where the permissions begin with 'l' and where the name is followed by a '-->' pointing to another directory location.
* Your symlink should point to '/home5/aqua/' followed by your Aquaculture Project Groupname (e.g. barra, edna, prawn, or gen).  
* For example, if you're in the eDNA project group, you should have a symlink which points to '/home5/aqua/edna/'.  Make a note of your symlink name, we will need it for next steps.
3. Now that you have the name of your symlink directory, we can proceed to configure your rsync script. Start by disconnecting from the hpc, use the 'exit' command
```         
exit
```     
4. Return to your Cygwin HOME account
```
cd $HOME
```
5. Create a folder on your Desktop to contain the files that you want to synchronise to the HPC.
```        
mkdir Desktop\jcXXYYYY_AQSYNC
```
6. Create and open your 'rsync' script with nano
```
nano \cygdrive\c\cygwin64\bin\rsync.sh
```       
7. Type the following lines into your shell script exactly (substituting your jc numberr and the name of your SYMLINK directory in the second argument):
```
#!/bin/bash
rsync -avz \cygdrive\c\Users\jcXXYYYY\Desktop\jcXXYYYY_AQSYNC HPC:/homeN/XX/jcXXYYYY/MYSYMLINK
```  
* In the above command, your directory path will start with 'homeN', where 'N' is a number between 1 and 6. If you can't remember which 'home' your HPC count resides in.  
Simply log-in to the HPC with the command `ssh HPC` then run the command `pwd` to display the full directory path of your HPC home.    
10. To exit nano and save this file:
* CTRL+X
* Then type 'y' to confirm the file name and press ENTER again

### <a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/winautomate/">Final Step: Automate File Synchronisation</a>
