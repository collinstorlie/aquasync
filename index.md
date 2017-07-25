---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
# Access & Sync Data To the JCU Aquaculture Shared HPC Drive

This is a step-by-step walkthrough guide for connecting to the HPC to store and automatically backup your research data.  It is partitioned into sections, and provides separate instructions for Mac and Windows PC Users.
Many commands below will contain directory paths which reference your user name for your computer, i.e. your JC number.
Whenever you see a reference to jcXXYYYY within a command in the following documentation, ensure to substitute your JC number instead.

## Connecting to the HPC with FileZilla (For Mac and Windows PC Users)

FileZilla is an SFTP (Secure File Transfer Protocol) Client, used to transfer files between two computers over a secure internet connection.  FileZilla has a GUI interface, allowing you to simply 'drag & drop' files between your computer and the HPC.
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

<p><a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/macconnect/">Step 1: Configure Your Mac To Connect To The HPC</a></p>
<p><a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/mackeys/">Step 2: Generate Keys & Secure Your Files</a></p>
<p><a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/macsync/">Step 3: Synchronise Files Between Your Mac & The HPC</a></p>
<p><a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/macautomate/">Step 4: Automate File Synchronisation Between Your Computer & The HPC</a></p>
     
## Configuring Your Windows Computer

<p><a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/wincygwin/">Step 1: Install Cygwin Terminal Emulator</a></p>
<p><a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/winconnect/">Step 2: Configure Your Windows PC To Connect To The HPC</a></p>
<p><a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/winkeys/">Step 3: Generate Keys & Secure Your Files</a></p>
<p><a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/winsync/">Step 4: Synchronise Files Between Your Windows PC & The HPC</a></p>
<p><a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/winautomate/">Step 5: Automate File Synchronisation Between Your Computer & The HPC</a></p>

If you are discover any errors in this documentation, please raise an issue on GitHub or contact collin.storlie@jcu.edu.au.

Please note I (Collin Storlie) will be on vacation from July 29th - August 25th 2017. 

Any issues during that period should be directed to Andrew Norton (ITC Dept.) via a ServiceNow request.
