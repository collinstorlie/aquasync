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
