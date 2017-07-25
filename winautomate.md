---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: default
---
### Automate File Synchronisation Between Your Windows PC & The HPC

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









