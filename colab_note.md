## Watch usage gpu

```
watch -n0.1 nvidia-smi
```

## Clear gpu memory on colab wihtout restart session
First: 
```
sudo fuser -v /dev/nvidia*
```

Second: replace the x with the PID the PID from first step:
```
sudo kill -9 x
```

Third: From the task Ram/Disk, chosing connect to the hosted runtime.

## Setup github to clone private respo

```
import os
from getpass import getpass
import urllib

user = input('User name: ')
password = getpass('Password: ')
password = urllib.parse.quote(password) # your password is converted into url format
repo_name = input('Repo name: ')

cmd_string = 'git clone https://{0}:{1}@github.com/{0}/{2}.git'.format(user, password, repo_name)

os.system(cmd_string)
cmd_string, password = "", "" # removing the password from the variable
```
