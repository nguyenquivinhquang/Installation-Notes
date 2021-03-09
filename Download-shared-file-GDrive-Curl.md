# Downloading Shared Files on Google Drive Using Curl

When the shared files on Google Drive is downloaded, it is necessary to change the download method by the file size. The boundary of file size when the method is changed is about **40MB**.

### File size < 40MB
#### CURL
~~~bash
filename="### filename ###"
fileid="### file ID ###"
curl -L -o ${filename} "https://drive.google.com/uc?export=download&id=${fileid}"
~~~

### File size > 40MB
When it tries to download the file with more than 40MB, Google says to download from following URL.

~~~html
<a id="uc-download-link" class="goog-inline-block jfk-button jfk-button-action" href="/uc?export=download&amp;confirm=####&amp;id=### file ID ###">download</a>
~~~

Query included ``confirm=####`` is important for downloading the files with large size. In order to retrieve the query from the HTML, it uses [``pup``](https://github.com/EricChiang/pup).

~~~bash
pup 'a#uc-download-link attr{href}'
~~~

And ``sed`` is used to remove ``amp;``.

~~~bash
sed -e 's/amp;//g'`
~~~

So curl command is as follows.

#### CURL
~~~bash
filename="### filename ###"
fileid="### file ID ###"
query=`curl -c ./cookie.txt -s -L "https://drive.google.com/uc?export=download&id=${fileid}" | pup 'a#uc-download-link attr{href}' | sed -e 's/amp;//g'`
curl -b ./cookie.txt -L -o ${filename} "https://drive.google.com${query}"
~~~

The value of ``confirm`` is changed every time. So when the value of ``confirm`` is retrieved, the cookie is saved using ``-c ./cookie.txt``, and when the file is downloaded, the cookie has to be read using ``-b ./cookie.txt``.

By using this method, you can download files with the size of over 1 GB.

Reference: [http://qiita.com/netwing/items/f2a595389f39206235e8](http://qiita.com/netwing/items/f2a595389f39206235e8)

## Update at March 21, 2019
### sample script
This is a simple bash script.

```bash
#!/bin/bash
fileid="### file id ###"
filename="MyFile.csv"
curl -c ./cookie -s -L "https://drive.google.com/uc?export=download&id=${fileid}" > /dev/null
curl -Lb ./cookie "https://drive.google.com/uc?export=download&confirm=`awk '/download/ {print $NF}' ./cookie`&id=${fileid}" -o ${filename}
```

Reference: [https://stackoverflow.com/questions/48133080/how-to-download-a-google-drive-url-via-curl-or-wget/48133859#48133859](https://stackoverflow.com/questions/48133080/how-to-download-a-google-drive-url-via-curl-or-wget/48133859#48133859)

### CLI tool
If you will use a CLI tool, please use this. This is a CLI tool to download shared files and folders from Google Drive. For large file, the resumable download can be also run.

- [https://github.com/tanaikech/goodls](https://github.com/tanaikech/goodls)

