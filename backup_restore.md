## Install and configure infrastructure with Ansible:

### Install with one play

    ansible-playbook infra.yaml
    Use this command if you dont know what is the issue!

    If you know the issue is with the backup, it is mandatory to first run the backup step (mentioned on the bottom) and only after you should run the play!

## Restore MySQL data from the backup:
    Since backup user is not configured properly, the easiest way is to first login to the user backup, then use duplicity to get
    the backup from the remote server. The tail command will ensure you, that the backup exist and we can use it.
    The last step will recover the database and drop the old one.
    
    1. sudo su - backup
    2. duplicity --no-encryption restore rsync://sandr0o@backup//home/sandr0o/mysql/home/backup/restore/agama
    3. exit
    4. sudo su -
    5. mysql agama < /home/backup/restore/agama
    ##works

## Restore InfluxDB data from the backup:


    In the first step, you have to become backup user in order to make the second step, which is to download backup from the remote server.
    In the thirs step, you have exit user backup, to login to user root in step 4. Then, after stopping the telegraf service, influxdb will drop its database.
    In step 7, we restore the EMPTY database with the file that we downlaoded, then start the telegraf service in the last step.

    1. sudo su - backup
    2. duplicity --no-encryption restore rsync://sandr0o@backup//home/sandr0o/influxdb/home/backup/influxdb/
    3. exit
    4. sudo su -
    5. service telegraf stop
    6. influx -execute 'DROP DATABASE telegraf'
    7. influxd restore -portable -database telegraf /home/backup/influxdb
    8. service telegraf start
    ##works
