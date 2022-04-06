# Add file 

1: Add specific file
```
git add PATH
```

2: Add all your change files

```
git add .
```

# Commit

```
git commit -a -m 'commit message'
```

# Pull 

```
git pull
```

# Push
```
git push
```

# Check commit history 
``` 
git log
```
To exit when in log, press Q or ctrl + z

# Show modified files in working directory, staged for your next commit
```
git status
```

# Show current branch
```
git branch
```

# Switch to branch A
```
git checkout A
```

# Merge branch A to branch B

First, switch to branch A
```
git checkout A
```

Second, pull file
``` 
git pull
```

Third, switch to branch B
```
git checkout B
```

Fourth, merge
```
git merge A
```
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
