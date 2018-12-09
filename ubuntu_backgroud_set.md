## ubuntu 系统美化

---
> ubuntu自带锁屏壁纸太单调，经常看难免会视觉疲劳，故通过培养桃红脚本开机自动下载当日的Bing壁纸并设为锁屏界面，记录设置过程。

### 1. 将图片下载到本地

python写一个图片爬虫，原理i比较简单，代码如下：

```python3
#!/usr/bin/python3
import json
import os
import urllib.request
import datetime

date = datetime.datetime.now().strftime('%Y-%m-%d')
json_url = "https://www.bing.com/HPImageArchive.aspx?format=js&idx=0&n=1&mkt=en-US"
bing_url = "https://www.bing.com"
HOME = os.path.expandvars('$HOME')+"/"
json_file = HOME+".bing.json"
directory = HOME+"picture/desktop"
picture = directory+"/"+date+".jpg"
if not os.path.exists(directory):
    os.makedirs(directory)
if not os.path.exists(picture):
    # get the json file and hide the file
    urllib.request.urlretrieve(json_url, json_file)
    # open the file and import json string
    with open(json_file, "r") as f:
        bing_json = json.load(f)
    url_append = bing_json['images'][0]['url']
    url = bing_url+url_append
    # get picture
    urllib.request.urlretrieve(url, picture)
    # change screen saver
    cmd = "gsettings set org.gnome.desktop.screensaver picture-uri file:"+picture
    #"gsettings set org.gnome.desktop.background picture-uri "file:/home/leon/pic/111.jpg"
    os.system(cmd)

```

### 2. 设置开机启动

##### 1.新建脚本运行python文件

```
#!/bin/sh 
### BEGIN INIT INFO # Provides:             start_test.sh 
# Required-Start:       $all 
# Required-Stop:        $all 
# Default-Start:        2 3 4 5 
# Default-Stop:         0 1 6 
# Short-Description:    starts the  start_test.sh 
# Description:          starts  start_test.sh 
#                       for security. 
### END INIT INFO mkdir /root/start_test456

python3 /home/python/start.py
``` 

##### 2. 拷贝到/etc/init.d/下

```
sudo cp ./start_test.sh /etc/init.d/
```

##### 3. 设置权限

```
sudo chmod +x start_test.sh
```

##### 4. 将脚本添加到启动脚本
执行如下指令，在这里90表明一个优先级，越高表示执行的越晚

```
cd /etc/init.d/
sudo update-rc.d start_test.sh defaults 90
```

----
ok,大功告成，以后每天开机就会自动下载Bing图片并设置为锁屏壁纸了。