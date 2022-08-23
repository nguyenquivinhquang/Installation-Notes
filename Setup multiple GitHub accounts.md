# Step 1 – Generate New SSH keys

First of all, check for all the available SSH keys in your account. Type: ls -l ~/.ssh to list all key pairs, So you won’t overwrite any key with below commands.

Let’s create first key pair by typing below command.

```
ssh-keygen -t rsa -C "your-email" 
```

+ Execute above command with your email address.
+ Set a new file path to prevent override existing file as: /home/rahul/.ssh/id_rsa_github_proj1
+ Press enter for passphrase and confirm passphrase to keep it empty
+ Your key is ready.