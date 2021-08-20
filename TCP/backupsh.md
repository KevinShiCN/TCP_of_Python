```bash
#!/bin/bash

# 获取IP地址
ipadd=$(ip add | sed -n '9p' | awk -F '[ /]+' '{print $3}')


# 获取日期
filename=$(date +"%Y-%m-%d")

mulu=${ipadd}_${filename}
# 创建文件夹
mkdir -pv /backup/$mulu &> /dev/null

#对需要的内容进行打包
tar -czf /backup/${mulu}/test.tar.gz /etc/passwd &> /dev/null

# 发送数据
expect rsync.exp $mulu


# 保留七天以内的数据
find /backup -mtime +7 -delete
```

