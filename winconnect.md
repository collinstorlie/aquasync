---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
### Configure Your Windows Computer To Connect To The HPC

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
        
### <a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/winkeys/">Next Step: Synchronise Files</a>
