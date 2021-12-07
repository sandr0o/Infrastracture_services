#Backup MySQL
1)sudo -su backup
2)mysqldump agama > /home/backup/mysql/agama.sql
3)duplicity --no-encryption full /home/backup/mysql/ rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ For full backup
4)duplicity --no-encryption incremental /home/backup/mysql/ rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ For incremental backup
#Restore MySQL
1)mysql agama < /home/backup/mysql/agama.sql
2)duplicity --no-encryption restore rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ /home/backup/restore/ Download backup
#Backup InfluxDb
1)sudo -su backup
2)rm -rf /home/backup/influxdb/*; influxd backup -portable -database telegraf /home/backup/influxdb
3)duplicity --no-encryption full /home/backup/influxdb/ rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ Full backup
4)backup duplicity --no-encryption incremental /home/backup/influxdb/ rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ Incremental backup
#Restore InfluxDB
1)service telegraf stop
2)influxd restore -portable -database telegraf /home/backup/influxdb
