---
title: shell notes
date: 2016-09-02 08:50:18
tags: 工具
---

### Invoking the shell
bash [options] [arguments]
--help
--init-file file
use file as the startup file instead of ~/.bashrc
--noprofile
do not read /etc/profile or any of the personal startup files.
--norc
-,--
arguments (script name)$0,[$1,$2...]

### Exit Status
$?


### Brace Expansion
 - pre{start..end[..incr]}post
 - pre{X,Y[,Z...]}post

### Quoting
Quoting disables a character's special meaning and allows it to be used iterally.


### Jobs control
ctrl+z,bg,fg

### Screen
Ctrl-a : resize 70

### Brew service
brew services start mysql

To connect run:
    mysql -uroot

To have launchd start mysql now and restart at login:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  mysql.server start

### To schedule a task that runs every N minutes ON WINDOWS
```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]

two examples

schtasks /create /sc minute /mo 20 /tn "Security Script" /tr \\central\data\scripts\sec.vbs

schtasks /create /tn "Security Script" /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k

```

### Set TimeZone on ubuntu

```
sudo dpkg-reconfigure tzdata
```

### yesterday on bash

```
ddate=`date -d "yesterday" '+%Y-%m-%d'`
echo $ddateta

#onpython
from datetime import datetime , timedelta
import pytz

#get yesterday as fetching parameters
today = datetime.now(pytz.timezone('Asia/Shanghai'))

yesterday = today - timedelta(1)
# ddate = yesterday.astimezone(to_zone).strftime("%Y-%m-%d")

ddate = datetime.strftime(yesterday, "%Y-%m-%d")

```

### Port
```
netstat -ntlp | grep LISTEN
```
 
Login Shells:

  * [Bash Configurations Demystified](http://dghubble.com/blog/posts/.bashprofile-.profile-and-.bashrc-conventions/).
  * [Bash Startup Files](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html).


Daily shells:
```
find . -name *.xml | xargs grep error
grep -i 'amazon\.(de\|com)'  --color
nohup sh -c 'ls -f images/ | wc -l ' 2>&1 >> images.num &
nohup find /images/ -type f -mtime +5 -delete &
```

Git Flow
```
git log --author="someone" --reverse
```

### Maven

```bash
mvn help:effective-pom > eff.txt

mvn archetype:generate

```

### Hadoop
```sql
describe formatted  user_profile;
```