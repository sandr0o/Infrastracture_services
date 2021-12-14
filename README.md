<<<<<<< HEAD
<h1>Backup SLA</h1> <br>
<h1>Introduction</h1> <br>
<p>
A service-level agreement is a commitment between a service provider and a client. Particular aspects of the service – quality, availability, responsibilities are agreed between the service provider and the service user    
</p>
<h1>Backup objectives</h1>
<p>
Backing up the services is crucial part of the strong company. We provide best practices to backup services such as Database servers [InfluxDB][MySQL] and Ansible repo.
</p>
<h1>RPO -- (recovery point objective)</h1>
<p>
our backup system is both incremental and full. Incremental backup system, restores data before the point where the loss has
happened. Full backup provides full backup at every specific time.     
</p>                                                  
<h1>Versioning and retention -- (how many backup versions are stored and for how long)</h1>
<p>
As it is said, incremental backup system has possibility to restore the data fully before the "loss point". Older intervals are stored for 1                          month                                                       
</p>
<h1>Usability checks -- (how is backup usability verified)</h1>
<p>
We create daily test database backup on your server. The data is encrypted and sent over secure channel to our data center.     
You have possibility to restore the data for checks and usability.
</p>
<h1>Restoration criteria -- (when should backup be restored)</h1>
<p>
Restoring the data is first step when hardware or software failure happens and system shutsdown.   
</p>
<h1>RTO -- (recovery time objective)</h1>
<p>
RTO is maximum time objective that company is capable of restoring the data. The RTO is not quite specified   
and is more or less approximate. The reason we only restore databases and ansible repository it requires    
from 4-5 hours or more if we are dealing with giant data
</p>
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
