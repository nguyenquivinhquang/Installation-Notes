# 1. Set up 2-factor authentication on Git
These are instructions for setting up git to authenticate with GitHub when you have 2-factor authentication set up. This authentication should be inherited by any GUI client you are using. These are intentionally brief instructions, with links to more detail in the appropriate places.

1. Download and install the [git command-line client](https://git-scm.com/download) (if required).

2. Open the git bash window and [introduce yourself to git](http://happygitwithr.com/hello-git.html) (if required):
		Remove old stuffs
	```
    git config --global --unset-all user.name
    git config --global --unset-all user.email
	```
    ```
    git config --global user.name 'Firstname Lastname'
    git config --global user.email 'firstname.lastname@gov.bc.ca'
    ```

3. Turn on the credential helper to cache your credentials (so you only need to do this once):

    a. Windows ([more detailed instructions here](https://help.github.com/articles/caching-your-github-password-in-git/#platform-windows)):
    ```
    git config --global credential.helper wincred
    ```
    
    b. Mac ([more detailed instructions here](https://help.github.com/articles/caching-your-github-password-in-git/#platform-mac)): 
    ```
    git config --global credential.helper osxkeychain
    ```

4. [Set up a personal access token](https://help.github.com/articles/creating-an-access-token-for-command-line-use) for accessing GitHub repositories - I recommend giving it `gist`, `repo`, and `user` scope. *Make sure you copy the token now as you won't be able to later*

5. Create a test repository in the [bcgov organization](https://github.com/bcgov).

6. Clone the repository on the command line (terminal/git bash window):
    ```
    git clone https://github.com/bcgov/[my-test-repo]
    ```

7. Make any change you want in your local repository. Eg., make or edit your `README.md` file.

8. Add, commit, and push your changes:
    ```
    git add README.md
    git commit -m "Edit README"
    git push -u origin master
    ```

    At this point you'll be asked for your username and password. Enter your username, then in the password prompt *paste your Personal Access Token you created in step 3*. (Note: in the Windows git bash shell, paste with Shift+Insert or right-click -> paste)

9. Now push AGAIN.
    ```
    git push
    ```
    
    You should NOT be asked for your username and password, instead you should see `Everything up-to-date`.

    Rejoice and close the shell.  If your test repository isn't important, you can delete it from the bcgov GitHub account.




# 2. Cache git account on current host
 
**A More Secure Method: Caching**



You can also have Git store your credentials permanently using the following:

	git config credential.helper store

or we can use git-credential-store to cache our username and password for a time period. Simply enter the following in your CLI (terminal or command prompt):
	
	git config --global credential.helper cache


You can also set the timeout period (in seconds) as such:

	git config --global credential.helper 'cache --timeout=3600'


**If you want your password to be forgotten by Git** 

	git config --unset-all credential.helper
	git config --global --unset-all credential.helper
	git config --system --unset-all credential.helper

