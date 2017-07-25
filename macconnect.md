---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---

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
     
### <a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/mackeys/">Next Step: Generate Keys</a>
