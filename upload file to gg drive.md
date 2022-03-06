## Step 1: Download the latest release from Github
```
wget https://github.com/prasmussen/gdrive/releases/download/2.1.1/gdrive_2.1.1_linux_386.tar.gz
```

## Step 2: Unzip the Archive

```
tar -xvf gdrive_2.1.1_linux_386.tar.gz
```

## Step 3: Perform Authentication
```
./gdrive about
```

In the console you will see an authentication URL that you can click and open in the browser to obtain an auth token.

When the URL opens in the browser, just select your account and copy the auth token that is displayed on the next screen.


## Step 4: Upload the File to Google Drive

```
./gdrive upload /home/documents/file_name.zip
```
