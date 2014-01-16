---
layout: post
title: 打包并加密网站文件脚本
category: Techs
tags: [shell,]
keywords: shell
description: 打包并加密网站文件脚本
---

```
#!/bin/bash

#设置部分
WEB_DIR="/var/www"
BACKUP_DIR="/root/backup"
MYSQL_USER="root"
MYSQL_PASS="123456"
DES3_ZIP_PW="iwend"
DES3_ZIP="des3"  #zip,或des3有效，采用zip软件加密或des3加密，des3需tar配合打包压缩率高。
WEB_DATA="web_$(date +%Y%m%d)"
MYSQL_DATA="mysql_$(date +%Y%m%d)"

#本地备份目录
if [ ! -d $BACKUP_DIR ]
    then
        mkdir -p "$BACKUP_DIR"
fi

cd $BACKUP_DIR

# 备份所有数据库
# 导出需要备份的数据库，清除不需要备份的库
mysql -u$MYSQL_USER -p$MYSQL_PASS -B -N -e 'SHOW DATABASES' > databases.db
sed -i '/performance_schema/d' databases.db
sed -i '/information_schema/d' databases.db
sed -i '/mysql/d' databases.db

for db in $(cat databases.db)
 do
   mysqldump -u$MYSQL_USER -p$MYSQL_PASS ${db} | gzip -9 - > ${db}.sql.gz
done
rm databases.db

if [ "$DES3_ZIP" = "des3" ]
then
    #压缩数据库并加密
    tar -zcvf - *.sql.gz |openssl des3 -salt -k $DES3_ZIP_PW | dd of="$MYSQL_DATA".des3
    rm *.sql.gz
    #压缩网站文件并加密
    tar -zcvf - $WEB_DIR |openssl des3 -salt -k $DES3_ZIP_PW | dd of="$WEB_DATA".des3
else
    zip -rP $DES3_ZIP_PW "$MYSQL_DATA".zip *.sql.gz
    rm *.sql.gz
    zip -rP $DES3_ZIP_PW "$WEB_DATA".zip $WEB_DIR
fi
```