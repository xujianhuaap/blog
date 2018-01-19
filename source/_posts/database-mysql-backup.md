---
title: mysql-backup
date: 2017-11-26 19:43:15
tags: database_mysql_backup
---
```
1. 进行备份
	mysqldump -u root -p database_name > backup.sql
2. 导入备份
　　　　mysql -u root -p database_name < backup.sql
```
