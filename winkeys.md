---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
### Generate Keys & Secure Your Files

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
* Make sure you paste your PUBLIC key here and where the above command say "Contents of Your PUBLIC Key File".  It must be surrounded by double-quotes.  You can paste text into the Command Prompt by right-clicking within the window and selecting 'Paste'.
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
chmod 700 .ssh
```      
* Set the permissions on your 'known\_hosts' file 
```
chmod 600 .ssh/known_hosts
```     
* Set the permissions on your 'authorized\_keys' file 
```        
chmod 644 .ssh/authorized_keys
```

### <a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/winsync/">Next Step: Generate Keys</a>
