---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
Please follow these step-by-step instructions, if you're having difficulty contact collin.storlie@jcu.edu.au for assistance.
You will notice some lines in these instructions are in code blocks (highlighted in blue):
```
pwd
```  
These instructions can be pasted directly into your Cygwin Terminal.  
Please note that some commands reference file/directory paths specific to your computer, these often consist of a user name, and will be different for each person.  

For example my HPC home account and Windows home are respectively:
```
/home1/15/jc152199/
C:\Users\jc152199\
```  
Your paths will be based on your user id too, don't forget to substitute them where appropriate.
Also note that windows paths use '\\' slashes instead of '/' slahes.

Now let's get started by installing Cygwin, which is a Terminal Emulator for Windows.  Installation of Cygwin is NECESSARY if you wish to synchronise files between your Windows PC and the HPC.

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

### <a class="post-link" href="{{ site.baseurl | replace:'http:','https:' }}/{{ baseurl }}/winconnect/">Next Step: Connect To The HPC</a>
