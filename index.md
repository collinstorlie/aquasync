---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
# Accessing the JCU Aquaculture Shared HPC Drive

This is a step-by-step walkthrough guide for connecting to the HPC to store and automatically backup your research data.
Many commands below will contain directory paths which reference your user name for your computer, i.e. your JC number.
Whenever you see a reference to jcXXYYYY within a command in the following documentation, ensure to substitute your JC number instead.

## Connecting to the HPC with FileZilla

FileZilla is an SFTP (Secure File Transfer Protocol) Client, used to transfer files between two computers over a secure internet connection.
You can download Filezilla Client from their website <a href='https://filezilla-project.org/download.php'>here</a>, make sure to select the appropriate version for your computer operating system.
Once you've installed FileZilla Client, you'll need to configure it to connect to the HPC.  Follow these short steps:

1. From the Menu Bar select File --> Site Manager

2. Click 'New Site' at the bottom-left of the window.

3. Give the entry a name, perhaps 'HPC'

4. Add configuration details to the right panel, when you're finished click 'OK':
* Host: zodiac.hpc.jcu.edu.au
* Port: 8822
* Protocol: SFTP
* Logon Type: Ask For Password
* User: jcXXYYYY

5. Your configuration is now saved, you can connect to the HPC by returning to the Site Manager, selecting 
the 'HPC' connection, and clicking 'Connect'

6. If you've never connected to the HPC before, you'll see a message warning you that you're connecting to a new computer, click 'OK' or 'yes' to proceed.

7. You'll then presented with directory listings, the directory tree on the left is your local machine.  The directory tree on the right is your HPC account.

8. To transfer files and folders between your computer and the HPC, simply drag and drop between the two windows.
* Please note, when you initially connect to the HPC via Filezilla, you'll automatically be placed in your HPC 'HOME' directory, make a note of this location for later.

## Configuring Your Mac Computer

Please follow these step-by-step instructions, if you're having difficulty contact collin.storlie@jcu.edu.au for assistance.

You will notice some lines in these instructions are in code blocks (highlighted in blue):

```
pwd
```
    
These instructions can be pasted directly into your Mac Terminal.
Please note that some commands reference file/directory paths specific to your computer, these often consist of a user name, and will be different for each person.  

For example my HPC home account and Mac home are respectively:

```
/home1/15/jc152199/
/Users/jc152199/
```
    
Your paths will be based on your user id too, don't forget to substitute them where appropriate.

### Configure your Mac to connect to the HPC

1.  Open the Terminal, found in Applications -> Utilities -> Terminal
2.  Determine the HOME directory of your computer and make a note of it.
```
echo $HOME
```
3.  List the files/directories in your HOME directory, look for the directory labelled '.ssh'
```
ls -a .ssh  
```
4.  If this command returns the error 'No such file or directory', then we need to create the directory.  If you don't see this warning, then you can proceed to Step 6
5.  Create a directory named '.ssh'
```
mkdir .ssh
```       
6. Navigate to your .ssh directory 
```
cd .ssh
```      
7. Now we have the directory to place our 'keys' in, keys allow us to securely connect to other computers (including the HPC).
Use the following command to create your keys:
```        
ssh-keygen -b 4096 -t rsa -f HPC
```       
8. This command creates two files, a public and private key of type 'rsa' with 4096 byte encryption, files will be named HPC.pub and HPC respectively.
* HPC is the 'private key' which will be kept securely on your local computer
* HPC.pub is the 'public key' which needs to be placed in '.ssh/authorized_keys' in your HPC HOME account
9. Start by creating a the .ssh config file with the command `touch`
```
touch config
```
10. Now we can use the text editor 'nano' to add some connection information to the config file.  Nano will open within your Terminal window.
11. To open nano and start editing your 'config' file, use the command
```
nano config
```
12. Insert the following lines exactly (substituting your JCU ID)
```
Host HPC
    HostName zodiac.hpc.jcu.edu.au
    User jcXXYYYY
    Port 8822
    IdentityFile ~/.ssh/HPC
```

13. To exit nano, and save your config file, do the following:
* CTRL+X To Save
* Type 'y' to confirm the file name as 'config' and then press ENTER
14. Before we can connect to the HPC with our shiny new keys, we'll need to add our PUBLIC key to a file in your HPC home account.
Your key is just a long text string, let's copy it onto your clipboard so that we can add it to the HPC next.
```
pbcopy < HPC.pub
```
     
### Connect to the HPC and Add Your Key to the 'authorized_keys' File

1. Connect to the HPC using ssh 
```
ssh HPC
```    
* When you FIRST connect you'll be presented with a warning 'The authenticity of host can't be established.  Are you sure you want to continue connecting?'
* This is your local computer warning you that you're about to connect to another computer it hasn't connected to before, press 'yes' then ENTER to proceed
2. Navigate to the '.ssh/' directory using `cd`
```
cd .ssh
```
3. Now, we can paste the contents of your PUBLIC key file to 'authorized\_keys' file here, using the following command:
```
echo "Contents of Your Public Key" >> authorized_keys
```
* Make sure you paste your PUBLIC key here, it will be a very long string of alphanumeric characters.  It must be surrounded by double-quotes.  You can paste text into your Terminal with CTRL+V.
4.  Now close your connection to the HPC by typing the command `exit`
5.  Now you should be able to connect without a password using your ssh config file, so try to connect again using `ssh`
```
ssh HPC
```        
* If all is well, your connection to the HPC will proceed WITHOUT you needing to specify your password.
* Please note, that ANYONE who obtains your PRIVATE key will now be able to connect to the HPC as you without providing a password.
* You must protect your PRIVATE key.  To ensure maximum security for your PRIVATE key, we recommend using the following security settings for your .ssh directory and the files within it.
6. Return to your HOME account on YOUR computer (NOT THE HPC), simply input the command `cd` into your Terminal, then run the following commands:
* Set the permissions on your .ssh directory to 'read-only' for others 
```
chmod 744 .ssh/
```       
* Set the permissions on your PRIVATE key to 'read-only' for just you 
```
chmod 600 .ssh/HPC
```
7. Now reconnect to the HPC using `ssh HPC`, then run the following commands:
* Set the permissions on your .ssh directory 
```        
chmod 700 .ssh/
```    
* Set the permissions on your 'known\_hosts' file 
```        
chmod 600 .ssh/known_hosts
```     
* Set the permissions on your 'authorized\_keys' file 
```        
chmod 644 .ssh/authorized_keys
```

### Setup File Synchronisation Between Your Mac and the HPC

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
* First hold CONTROL and press ENTER
* Then type 'y' to confirm the file name and press ENTER again

### Automate File Synchronisation Between Your Computer and the HPC
1. This rsync script will now move files between your local computer and HPC HOME account, only moving files which have been newly created or modified since the last sync.
2. We will now configure the program 'cron' to schedule synchronisation as an automated task.  'cron' comes pre-installed on your Mac and is accessed via the Terminal.
3. We begin by adding a job to our 'crontab' file, to do this using the editor nano, run the following command:
```
env EDITOR=nano crontab -e
```
* Syntax for 'cron' is a bit bizarre, the basic structure of a 'cron' command is shown below:
`*     *      *      *     *      /path/to/your/rsync.sh`
* In the above example, each \* can be filled in with a time increment to determine the job frequency.  The final command is the name of the script to be executed at the chosen interval.
* Also note that the white-spaces between arguments in your crontab file are actually single TABS.
The image below may help to clarify 'cron' syntax.
<img src="img/crontab.png">
4. Type the following into your crontab file to schedule synchronisation for 12 noon every Wednesday
```
0     12      *     *      3      ~/Desktop/jcXXYYYY_AQSYNC/rsync.sh >> ~/Desktop/jcXXYYYY_AQSYNC/rsync.log 2>&1
```     
* NB: Remember, the 'white-spaces' are single TABS
* For more 'cron' examples, check out this <a href="https://tecadmin.net/crontab-in-linux-with-20-examples-of-cron-schedule/">website</a>.
5. To exit nano and save your new scheduled job:
* CTRL+X
* Then type 'y' and press ENTER
6. You're all finished, now your chosen directory will synchronise automatically to the HPC every Wednesday at noon.
* NB: Your computer must be powered ON for the scheduled 'cron' task to run.

## Configuring Your Windows Computer

Please follow these step-by-step instructions, if you're having difficulty contact collin.storlie@jcu.edu.au for assistance.
You will notice some lines in these instructions are in code blocks (highlighted in blue):
```
pwd
```  
These instructions can be pasted directly into your Mac Terminal.
Please note that some commands reference file/directory paths specific to your computer, these often consist of a user name, and will be different for each person.  

For example my HPC home account and Windows home are respectively:
```
/home1/15/jc152199/
C:\Users\jc152199\
```  
Your paths will be based on your user id too, don't forget to substitute them where appropriate.
Also note that windows paths use '\\' slashes instead of '/' slahes.

Now let's get started by installing Cygwin, which is a Terminal Emulator for Windows.

### Installing Cygwin

1. Determine if your OS is 32-bit or 64 bit:
* Open a File Explorer Window
* Right-click the 'This PC' icon, then select 'Properties'
* Look for the line labelled 'System Type', there you will see if your OS is 32 or 64-bit
2. Download the appropriate version of Cygwin for your OS <a href="https://cygwin.com/install.html">here</a>.
3. Once the executable file has downloaded, locate it (try looking in Downloads) and double-click it
4. Click 'Next' at the first window that appears
5. Select 'Install From Internet' and click 'Next'
6. Confirm the 'root' directory to install Cygwin at, it should be C:\\cygwin64\\, ensure that you select 'Install for All Users' and then click 'Next'
7. Confirm the location to download the additional packages we require, it should be C:\\Users\\jcXXYYYY\\Downloads, then click 'Next'
8. Confirm the default preferred internet connection as 'Direct Connection', then click 'Next'
9. At this point, if you don't have the most recent version of Cygwin the installer will display some warning messages.
If you see warning messages during this step, cancel the install, return to the <a href="https://cygwin.com/install.html">Cygwin download page</a> and get the newest version.
10. Once the installer finishes downloading the base Cygwin package (should only take a few seconds), a new window will open
that contains a list of other Cygwin packages we can install.  Please select the following additional packages to download at this point:
11. Use the search bar at the top and type in 'ssh' (don't press enter, or you will proceed with installation)
* Expand the 'Net' tab by clicking on the '+' symbol on the left
* We need to download ALL of these packages. Select the 'Skip' icon on the left for each package, a version number should appear in it's place.  Also, for each package, tick the box labelled 'src'.
12. Use the search bar at the top and type in 'rsync'
* Expand the 'Net' tab by clicking on the '+' symbol on the left
* There should be a single entry, select the 'Skip' icon on the left, a version number should appear in it's place.  Also, tick the box labelled 'src'.
13. Use the search bar at the top and type in 'nano'
* Expand the 'Editor' tab by clicking the '+' symbol on the left
* We need to download BOTH of these packages.  Select the 'Skip' icon on the left, a version number should appear in it's place.  Also, tick the box labeled 'src'.
14. Now that you've selected the appropriate packages to download, you can click 'Next' at the bottom-right to proceed.
15. A new window will appear labelled 'Resolving Dependencies', these are additional packages that Cygwin MUST download for our desired packages to work.
Ensure the tick box labelled 'Select Required Packages' is ticked, then click 'Next' to proceed.
16. A new window will appear showing the  progress of your Cygwin downloads and installation, it may take a few minutes ( ~ 5 - 10 ) to complete
17. Once finished, a new window will appear.  Select where you want your Cygwin launch icons to be placed, then click 'OK' to finish.

### Configure your Windows computer to connect to the HPC

1.  Double click the Cygwin icon, it should open a Command Prompt screen
2.  Create a directory called .ssh within the directory \\home\\
```
cd \home\
mkdir -p jcXXYYYY\.ssh
```
3.  Navigate to your new .ssh directory
```
cd jcXXYYYY\.ssh
```
4.  Now we have the directory to place our 'keys' in, keys allow us to securely connect to other computers (including the HPC).
Use the following command to create your keys:
```
ssh-keygen -t rsa -b 4096 -f HPC
```
5. This command creates two files, a public and private key of type 'rsa' with 4096 byte encryption, files will be named HPC.pub and HPC respectively.
* HPC is the 'private key' which will be kept securely on your local computer
* HPC.pub is the 'public key' which needs to be placed in '.ssh/authorized\_keys' in your HPC HOME account
6.  Create your known\_hosts file
```
touch known_hosts
```       
7. Create your ssh config file 
```
touch config
```      
8. Now we can use the text editor 'nano' to add some connection information to the config file.  Nano will open within your Terminal window.  To open nano and start editing your 'config' file, use the command
```
nano config
```
9. Insert the following lines exactly (substituting your JCU ID)
```
Host HPC
    HostName zodiac.hpc.jcu.edu.au
    User jcXXYYYY
    Port 8822
    IdentityFile \home\jcXXYYYY\.ssh\HPC
```    		
10. To exit nano, and save your config file, do the following:
* CTRL+X (to save)
* Type 'y' to confirm the file name as 'config' and then press ENTER
11. Before we can connect to the HPC with our shiny new keys, we'll need to add our PUBLIC key to a file in your HPC home account.
Your key is just a long text string, let's copy it onto your clipboard so that we can add it to the HPC next.  First display the contents of your public key within your Command Prompt Window
```
cat HPC.pub
```
12. You can use your mouse to highlight the long string of alphanumeric characters that comprise your key (it should begin with 'ssh-rsa' and end with your JC number and computer serial number).  To copy the text, right-click within the Command Prompt window and select 'Copy'.
        
### Connect to the HPC and Add Your Key to the 'authorized_keys' File

1. Connect to the HPC using ssh 
```
ssh HPC
```     
* When you FIRST connect you'll be presented with a warning 'The authenticity of host can't be established.  Are you sure you want to continue connecting?'
* This is your local computer warning you that you're about to connect to another computer it hasn't connected to before, press 'yes' then ENTER to proceed
2. Navigate to the '.ssh/' directory using `cd`
```
cd .ssh
```
3. Now, we can paste the contents of your PUBLIC key file to 'authorized\_keys' file here, using the following command:
```
echo "Contents of Your PUBLIC Key File" >> authorized_keys
```
* Make sure you paste your PUBLIC key here and where the above command say "Contents of Your PUBLIC Key File".  It must be surrounded by double-quotes.  You can paste text Command Prompt by right-clicking within the window and selecting 'Paste'.
4.  Now close your connection to the HPC by typing the command `exit`
5.  Now you should be able to connect without a password using your ssh config file, so try to connect again using `ssh`
```
ssh HPC
```      
* If all is well, your connection to the HPC will proceed WITHOUT you needing to specify your password.
* Please note, that ANYONE who obtains your PRIVATE key will now be able to connect to the HPC as you without providing a password.
* You must protect your PRIVATE key.  To ensure maximum security for your PRIVATE key, we recommend using the following security settings for your .ssh directory and the files within it.
6. Return to your CYGWIN HOME account on YOUR computer (NOT THE HPC), simply input the command `cd \home\jcXXYYYY\` into your Terminal, then run the following commands:
* Set the permissions on your .ssh directory to 'read-only' for others 
```
chmod 744 .ssh
```     
* Set the permissions on your PRIVATE key to 'read-only' for just you 
```
chmod 600 .ssh\HPC
```
7. Now reconnect to the HPC using `ssh HPC`, then run the following commands:
* Set the permissions on your .ssh directory 
```        
chmod 700 .ssh/
```      
* Set the permissions on your 'known\_hosts' file 
```
chmod 600 .ssh/known_hosts
```     
* Set the permissions on your 'authorized\_keys' file 
```        
chmod 644 .ssh/authorized_keys
```

### Setup File Synchronisation Between Your Windows PC and the HPC

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
<img src="img/symlink.png">
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

### Automate File Synchronisation Between Your Windows PC and the HPC

1. The final step is to use Windows Task Scheduler to automate file synchronisation between our PC and the HPC.
2. Go to Control Panel --> System \& Security --> Administrative Tools --> Task Scheduler
3. A new window will open, in the bar on the right-hand side, click 'Create Basic Task'.
4. Give your task a name and description, e.g.
* Name: HPC Sync
* Description: Synchronise files from my Desktop to the Shared AQ HPC Drive
5. When done, click 'Next'
6. Select the frequency of your scheduled task for whatever time interval you desire (I recommend 'weekly') then click 'Next'
7. Now select a time to start, being mindful that this scheduled task won't run if your computer is Powered Off or if you're not logged in.
8. Ensure the 'Recur Every' field is set to 1 week
9. Select a day for the synchronisation to occur on and click 'Next'
10. For the 'action' we want to select 'Start A Program', then click 'Next'
11. Now add the following configuration options:
* Set 'Program/script' to be:
```
C:\cygwin64\bin\bash.exe
```
* Set 'Add Arguments' to be:
```
-l -c rsync.sh
```
* Set 'Start In' to be:
```
C:\cygwin64\bin
```
12. Click 'OK' to save your scheduled task, and you're finished.









