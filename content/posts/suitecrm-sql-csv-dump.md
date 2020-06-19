---
title: "SuiteCRM SQL database dump to CSV files"
date: 2019-01-17T10:18:00
tags: ["formats", "guides", "linux", "servers", "snippets", "software"]
---

Recently I needed to dump our workplaces existing CRM database to a format that was easy to work with, namely CSV instead of the more likely SQL format.

To do this easily and quickly I developed a small script that would automatically grab the SQL servers credentials from your config file and then dump the files all from a single file. Your `config.php` usually resides in the `htdocs` folder on your server.

```
#!/bin/sh

# sqlHost=$(grep db_host_name config.php | cut -f4 -d\' | cut -f1 -d\:)
confFile=/opt/crm/apps/suitecrm/htdocs/config.php
sqlHost=127.0.0.1
sqlUser=$(grep db_user_name "$confFile" | cut -f4 -d\')
sqlPass=$(grep db_password "$confFile" | cut -f4 -d\')
sqlTable=$(grep db_name "$confFile" | cut -f4 -d\')
dumpDirectory=db_dump_$(date +%Y-%m-%d)

echo $sqlHost $sqlUser $sqlPass $sqlTable $dumpDirectory

mkdir "$dumpDirectory"
for table in $(mysql -h $sqlHost -u$sqlUser -p$sqlPass $sqlTable -sN -e "SHOW TABLES;"); do mysql -B -u$sqlUser -p$sqlPass $sqlTable -h $sqlHost -e "SELECT * FROM $table;" | sed "s/'/\'/;s/\t/\",\"/g;s/^/\"/;s/$/\"/;s/\n//g" > $dumpDirectory/$table.csv; done
find $dumpDirectory -type f -empty -delete
```
Make the script executable, run it and all being well you should have a folder containing all your csv files.

One point worth noting is the manual setting of `$sqlHost` as localhost doesn't seem to work with the `mysql` command so this may require changing if you're not running SQL and the script on the same machine.
