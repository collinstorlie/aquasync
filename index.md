---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
# Accessing the JCU Aquaculture Shared HPC Drive

This is a step-by-step walkthrough guide for connecting the HPC to store and automatically backup your research data.

## Connecting to the HPC w/ Filezilla

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
* User: Your JC Number (e.g. jcXXYYYY)

5. Your configuration is now saved, you can connect to the HPC by returning the Site Manager, selecting the 'HPC' connection, and clicking 'Connect'

6. If you've never connected to the HPC before, you'll see a message warning you that you're connecting to a new computer, click 'OK' or 'yes' to proceed.

7. You'll then presented with directory listings, the directory tree on the left is your local machine.  The directory tree on the right is your HPC account.

8. To transfer files and folders between your computer and the HPC, simply drag and drop between the two windows.

* Please note, when you initially connect to the HPC via Filezilla, you'll automatically be placed in your HPC 'HOME' directory, make a note of this location for later.

## Configuring Your Mac Computer

Please follow these step-by-step instructions, if you're having difficulty contact collin.storlie@jcu.edu.au for assistance.

You will notice some lines in these instructions are in code blocks (highlighted in blue):

    pwd
    
These instructions can be pasted directly into your Mac Terminal.
Please note that some commands reference file/directory paths specific to your computer, these often consist of a user name, and will be different for each person.  

For example my HPC home account and Mac home are respectively:

    /home1/15/jc152199/
    
    /Users/jc152199/
    
Your paths will be based on your user id too, don't forget to substitute them where appropriate.

### Configure your Mac to connect to the HPC

1.  Open the Terminal, found in Applications -> Utilities -> Terminal

2.  Determine the HOME directory of your computer and make a note of it.

        echo $HOME

3.  List the files/directories in your HOME directory, look for the directory labelled '.ssh'

        ls -a .ssh  

4.  If this command returns the error 'No such file or directory', then we need to create the directory.  If you don't see this warning, then you can proceed to Step 6

5.  Create a directory named '.ssh'

        mkdir .ssh
        
6. Navigate to your .ssh directory 

        cd .ssh
        
7. Now we have the directory to place our 'keys' in, keys allow us to securely connect to other computers (including the HPC).
Use the following command to create your keys:
        
        ssh-keygen -b 4096 -t rsa -f HPC
        
8. This command creates two files, a public and private key of type 'rsa' with 4096 byte encryption, files will be named HPC.pub and HPC respectively.

* HPC is the 'private key' which will be kept securely on your local computer

* HPC.pub is the 'public key' which needs to be placed in '.ssh/authorized_keys' in your HPC HOME account

9. Start by creating a the .ssh config file with the command `touch`

         touch config

10. Now we can use the text editor 'nano' to add some connection information to the config file.  Nano will open within your Terminal window.

11. To open nano and start editing your 'config' file, use the command

        nano config

12. Insert the following lines exactly (substituting your JCU ID)

        Host HPC
        	HostName zodiac.hpc.jcu.edu.au
        	User jc152199
    		Port 8822
    		IdentityFile ~/.ssh/HPC
    		
	
13. To exit nano, and save your config file, do the following:

* Hold CONTROL and press 'X' (to save)

* Type 'y' to confirm the file name as 'config' and then press ENTER

14. Before we can connect to the HPC with our shiny new keys, we'll need to add our PUBLIC key to a file in your HPC home account.
Your key is just a long text string, let's copy it onto your clipboard so that we can add it to the HPC next.

        pbcopy < HPC.pub
        
### Connect to the HPC, and add your key to the authorized_keys file

1. Connect to the HPC using ssh 

         ssh HPC
        
* When you FIRST connect you'll be presented with a warning 'The authenticity of host can't be established.  Are you sure you want to continue connecting?'

* This is your local computer warning you that you're about to connect to another computer it hasn't connected to before, press 'yes' then ENTER to proceed

2. Navigate to the '.ssh/' directory using `cd`

        cd .ssh

3. Now, we can paste the contents of your PUBLIC key file to 'authorized_keys' file here, using the following command:

        echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDuf8go52SCEgU9q2l1pXXi5pDgts2EtBQ6JqCJHI7bxafhQSH5NyKfYPioYpeWI8tkP+axwtthQphRksG+QNliCvkdo08l0TcNEmuocSuMD+0mgHKFPNZG82Jh5rL5oPewMR4E2dGpW6nhve+afYItL9iMNmKQD7Rg15SJTJ79xJUDXwCwtyrjRX4GcXeNkABM8DUipErQIQvSzx4DEDawTqAry0+xtAO5U43+Gh3LOJgosIDrMDzdORxvlupQKSHFIJm3ov3PhpKyyfNT90kEiXz6MJhU2A7fqg9x8JSqXgAVZxqIlPC1IgYiR1W+LF7ieY4VhBDKZVsMznCcvZ63jA+XWOmbMm5uVUNn8zohRVt5oiSgUeFP94UgF1ld4GG58P3gX65NJGLe15dJPYa6yfT5WZrga7qlE82TpIsTN0I3HT6kTKa5/SEtyfNQPu+qR/MsP96IJtr7uZEfrX8mtu60DC2rDc+2zZXME0BnTx8lV19V02v8hARokmr6bEQZv+1v4JMSqMmVcwleFU87euGb1DmYc1uC7Y9zoC/icqfsUsHVMdbGCYhq+ziyfwHphNh56+pM43vKc8id36DBKxc3J8LnZYt3y/qGHUfXJGc0yDE4WyKWPSP2NchhqCJrHcNX0TiSNqV+pufCZrvWWjwKQzGB4/TgSumAZ2h7Ow== jc152199@C02SY00TGTDX" >> authorized_keys

* In the example above, the long string of alphanumerals is my public key, make sure you paste your PUBLIC key here and not mine.  It must be surrounded by double-quotes.  You can paste text into your terminal by holding CONTROL and pressing 'V'.

4.  Now close your connection to the HPC by typing the command `exit`

5.  Now you should be able to connect without a password using your ssh config file, so try to connect again using `ssh`

         ssh HPC
         
* If all is well, your connection to the HPC will proceed WITHOUT you needing to specify your password.

* Please note, that ANYONE who obtains your PRIVATE key will now be able to connect to the HPC as you without providing a password.

* You must protect your PRIVATE key.  To ensure maximum security for your PRIVATE key, we recommend using the following security settings for your .ssh directory and the files within it.

6. Return to your HOME account on YOUR computer (NOT THE HPC), simply input the command `cd` into your Terminal, then run the following commands:

* Set the permissions on your .ssh directory to 'read-only' for others 

        chmod 744 .ssh/
        
* Set the permissions on your PRIVATE key to 'read-only' for just you 

        chmod 600 .ssh/HPC

7. Now reconnect to the HPC using `ssh HPC`, then run the following commands:

* Set the permissions on your .ssh directory 
        
        chmod 700 .ssh/
        
* Set the permissions on your 'known\_hosts' file 
        
        chmod 600 .ssh/known\_hosts
        
* Set the permissions on your 'authorized\_keys' file 
        
        chmod 644 .ssh/authorized\_keys

### Setup File Synchronisation Between Your Mac and the HPC

'rsync' is a program designed to synchronise files between two computers, it comes pre-installed on your Mac Computer and is accessed via the Terminal.
We will now setup 'rsync' to synchronise files between a chosen directory on your computer, and a directory in your HPC home account.
The directory we synchronise to in your HPC home account will be a 'symlink' to your project space within the Aquaculture Group shared HPC storage.
This means the files you synchronise will be viewable by other members of your Aquaculture Project Group.

1. To start, ensure you are logged into the HPC and in your HOME directory

        ssh hpc
        
        cd $HOME
        
2. The shortcut (symlink) to your Aquaculture Project space should already exist, you can confirm this by listing the files in your home directory 
        
        ls -laF
        
* Here is an example of what a symlink looks like when listed, please note the paths of your symlink will be different:

<img src="img/symlink.png">

* You should be presented with a long listing of files / directories within your HPC home.  

* Look for a line where the permissions begin with 'l' and where the name is followed by a '-->' pointing to another directory location.

* Your symlink should point to '/home5/aqua/' followed by your Aquaculture Project Groupname (e.g. barra, edna, prawn, or gen).  

* For example, if you're in the eDNA project group, you should have a symlink which points to '/home5/aqua/edna/'.  Make a note of your symlink name, we will need it for next steps.

3. Now that you have the name of your symlink directory, we can proceed to configure your rsync script. Start by disconnecting from the hpc, use the 'exit' command
         
         exit
         
4. Decide which local folder you'd like to synchronise to the HPC (can be anywhere on your computer)

5. For example, if you wanted to synchronise a directory called AQSYNC on your Desktop, you could first create the folder
        
        mkdir ~/Desktop/AQSYNC

6. Then navigate to it: 
        
        cd ~/Desktop/AQSYNC/
        
7. Create your rsync shell script using 'touch'
        
        touch rsync.sh
        
8. Use 'nano' to open and edit the file
        
        nano rsync.sh

9. Type the following lines into your shell script exactly (substituting your paths where appropriate):

        #!/bin/bash
        rsync -avz ~/Desktop/AQSYNC/ HPC:/home1/15/jc152199/MYSYMLINK
        
* NB: Please note the lack of a trailing slash in the second rsync script argument, this is intentional.

10. To exit nano and save this file:

* First hold CONTROL and press ENTER

* Then type 'y' to confirm the file name and press ENTER again

### Automate File Synchronisation Between Your Computer and the HPC

1. This rsync script will now move files between your local computer and HPC HOME account, only moving files which have been newly created or modified since the last sync.

2. We will now configure the program 'cron' to schedule synchronisation as an automated task.  'cron' comes pre-installed on your Mac and is accessed via the Terminal.

3. We begin by adding a job to our 'crontab' file, to do this using the editor nano, run the following command:

        env EDITOR=nano crontab -e

* Syntax for 'cron' is a bit bizarre, the basic structure of a 'cron' command is shown below:

        *     *      *      *     *      /path/to/your/rsync.sh

* In the above example, each \* can be filled in with a time increment to determine the job frequency.  The final command is the name of the script to be executed at the chosen interval.

* Also note that the white-spaces between arguments in your crontab file are actually single TABS.

The image below may help to clarify 'cron' syntax.

<img src="img/crontab.png">

4. Type the following into your crontab file to schedule synchronisation for 12 noon every Wednesday

         0     12      *     *      3      ~/Desktop/AQSYNC/rsync.sh >> ~/Desktop/AQSYNC/rsync.log 2>&1
         
* NB: Remember, the 'white-spaces' are single TABS

* For more 'cron' examples, check out this <a href="https://tecadmin.net/crontab-in-linux-with-20-examples-of-cron-schedule/">website</a>.

5. To exit nano and save your new scheduled job:

* Hold CONTROL and press X

* Then type 'y' and press ENTER

6. You're all finished, now your chosen directory will synchronise automatically to the HPC every Wednesday at noon.

* NB: Your computer must be powered ON for the scheduled 'cron' task to run.











