# 通过bark把ip推送到客户端脚本

```bash
#!/bin/bash
sleep 10
base="https://api.day.app/5548FSin9Qo8frWcFHexr7/"
ip=`hostname -I`
url=$base$ip
curl $url >/dev/null 2>&1
```