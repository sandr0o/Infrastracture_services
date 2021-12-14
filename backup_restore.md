<h1>Backup MySQL</h1>
<p>
1)sudo -su backup <br>
2)mysqldump agama > /home/backup/mysql/agama.sql<br>
3)duplicity --no-encryption full /home/backup/mysql/ rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ For full backup<br>
4)duplicity --no-encryption incremental /home/backup/mysql/ rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ For incremental backup<br>
</p>
<h1>Restore MySQL</h1>
<p>
1)mysql agama < /home/backup/mysql/agama.sql<br>
2)duplicity --no-encryption restore rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ /home/backup/restore/ Download backup<br>
</p>
<h1>Backup InfluxDb</h1>
<p>
1)sudo -su backup<br>
2)rm -rf /home/backup/influxdb/*; influxd backup -portable -database telegraf /home/backup/influxdb<br>
3)duplicity --no-encryption full /home/backup/influxdb/ rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ Full backup<br>
4)backup duplicity --no-encryption incremental /home/backup/influxdb/ rsync://sandr0o@backup.{{ domainName }}//home/sandr0o/ Incremental backup<br>
</p>
<h1>Restore InfluxDB</h1>
<p>
1)service telegraf stop<br>
2)influxd restore -portable -database telegraf /home/backup/influxdb<br>
</p>
