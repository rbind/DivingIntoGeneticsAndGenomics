---
title: Backup automatically with cron
author: Ming Tang
date: '2018-06-27'
slug: crontab-for-backup
categories:
  - data management
tags:
  - data
header:
  caption: ''
  image: ''
---

Data backup is an essential step in the data analysis life cycle. As shown in a pic below taken from [DataOne](https://www.dataone.org/best-practices).

![](/img/data-life-cycle.png)

There are so many important things you may want to back up: your raw/processed data, your code, and your dot configuration files. While for every project, I have git version control my scripts (not the data) and push it to github or gitlab to have a backup, big files can not be hosted on github or gitlab.

I usually back up my `projects` folder (containing all my scripts, raw data, processed data etc) to our high performance computing cluster in the `/rsch1` folder here at MD Anderson Cancer Center. IT stuff back up the contents there every week. In that essence, I have a copy in my local computer, a backup copy in the remote cluster and one more copy that IT stuffs backed up. I used to do `rsync -avhP ~/projects mdaris337:/rsch1/genomic_med/mtang1/tommy_mac_backup` once a week, but then sometimes I forget about it. I need a tool to do it every once a while for me. Here comes `cron` to help.

>cron is a Unix, solaris, Linux utility that allows tasks to be automatically run in the background at regular intervals by the cron daemon. 

>Crontab (CRON TABle) is a file which contains the schedule of cron entries to be run and at specified times. File location varies by operating systems, See Crontab file location at the end of this document.

commands for `crontab`:  

```bash
# It took me forever to quit vim :) so avoiding it now.
export EDITOR=nano ;to specify a editor to open crontab file.

crontab -e    Edit crontab file, or create one if it doesn’t already exist.
crontab -l    crontab list of cronjobs , display crontab file contents.
crontab -r    Remove your crontab file.
crontab -v    Display the last time you edited your crontab file. (This option is only available on a few systems.)
```

### crontab file
crontab syntax

```
*     *     *   *    *        command to be executed
-     -     -   -    -
|     |     |   |    |
|     |     |   |    +----- day of week (0 - 6) (Sunday=0)
|     |     |   +------- month (1 - 12)
|     |     +--------- day of        month (1 - 31)
|     +----------- hour (0 - 23)
+------------- min (0 - 59)
```


### Cron not working?

It happens to me that my cron job is not running. I googled around and found 
a comprehensive checking list that you can do to debug.

```
1. Is the Cron daemon running?
* Run ps ax | grep cron and look for cron.
* Debian: service cron start or service cron restart

2. Is cron working?
* * * * * /bin/echo "cron works" >> /file
Syntax correct? See above.

3. Is the command working standalone?
Check if the script has an error, by doing a dry run on the CLI
when testing your command, test as the user whose crontab you are editing, which might not be your login or root

4. Can cron run your job?
Check /var/log/cron.log or /var/log/messages for errors.
Ubuntu: grep CRON /var/log/syslog
Redhat: /var/log/cron

5. Check permissions
set executable flag on the command: chmod +x /var/www/app/cron/do-stuff.php
if you redirect the output of your command to a file, verify you have permission to write to that file/directory

6. Check paths
check she-bangs / hashbangs line
do not rely on environment variables like PATH, as their value will likely not be the same under cron as under an interactive session

7. Don't Suppress Output, while debugging
commonly used is this suppression: 30 1 * * * command > /dev/null 2>&1
re-enable the standard output or standard error message output
```

### My crontab file

```
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
MAILTO="tangming2005@gmail.com"

#rsync every Sunday 5am.
0 5 * * 0 rsync -avhP --exclude=".aspera" --exclude=".autojump" --exclude=".bash_history" --exclude=".bash_logout" --exclude=".cache" --exclude=".continuum" --exclude=".gem" --exclude=".gnome2" --exclude=".local" --exclude=".mozilla" --exclude=".myconfigs" --exclude=".oracle_jre_usage" --exclude=".parallel" --exclude=".pki" --exclude=".rbenv" --exclude=".Rhistory" --exclude=".rstudio" --exclude=".ssh" --exclude=".subversion" railab:.[^.]* ~/shark_dotfiles >> /var/log/rsync_shark_dotfiles.log 2>&1

0 5 * * 0 rsync -avhP --exclude=".aspera" --exclude=".autojump" --exclude=".bash_history" --exclude=".bash_logout" --exclude=".cache" --exclude=".continuum" --exclude=".gem" --exclude=".gnome2" --exclude=".local" --exclude=".mozilla" --exclude=".myconfigs" --exclude=".oracle_jre_usage" --exclude=".parallel" --exclude=".pki" --exclude=".rbenv" --exclude=".Rhistory" --exclude=".rstudio" --exclude=".ssh" --exclude=".subversion" mdaris337:.[^.]* ~/nautilus_dotfiles >> /var/log/rsync_nautilus_dotfiles.log 2>&1
```
### links for further reading

http://www.adminschoice.com/crontab-quick-reference  
https://stackoverflow.com/questions/22743548/cronjob-not-running

