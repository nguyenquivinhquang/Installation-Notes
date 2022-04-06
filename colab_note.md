## Watch usage gpu

```
watch -n0.1 nvidia-smi
```

## Clear gpu memory on colab without restart session
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

### First: 
Following the [link](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) to generate github token 

### Second:

```
import os
from getpass import getpass
import urllib

user = 'username'
token = 'user-token'
repo_name = 'user/respo-name'

cmd_string = 'git clone https://{0}:{1}@github.com/{2}.git'.format(user, token, repo_name)
print(cmd_string)
os.system(cmd_string)
cmd_string, token = "", "" # removing the password from the variable
```
