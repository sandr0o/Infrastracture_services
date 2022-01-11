# Backup - What, When, How

Our Infrastucture currently contains the following servers:

* Web Servers
* App Servers
* Database Servers
* DNS Servers
* Monitoring Servers
* Ansible repository

The purpose of this document is to explain the backup procedure of the mentioned infrastucture
## Backup Coverage

Three action will be taken in order to backup our infrastucture and restore the operation of our services.

### 1. Data Backups

Data backup will be idintified by two types of services: User-critical data, data that requires manual configuration.

#### Services which fall under this specification:

* MySQL Databases, Influx Databases - stores user data, which is mission-critical
* Grafana, Prometheus - manually configured, **not** mission-critical

### 2. Git storage

The Ansible repository is the highest priority, since it will provide everything that is necessary for the backup (e.g. commands, this document). It is stored on git and on management nodes.
This repository is updated after every working release.

### 3. Redeployment through Ansible repository

There are some services which although mission-critical, do not require any data backups and can be restored from the Ansible codebase (e.g. Bind).

#### Servers which fall under this specification:
* DNS Servers
* Web Servers
* App Servers
* Monitoring Servers:
  * Prometheus
  * Telegraf
  * InfluxDB


## Backup Recovery Point Objective (RPO)

Acceptable time window of customer data loss is 24 hours.

In case of the other data source (Manually configured Grafana) - acceptable time window grows considerably, as it does not see changes nearly as often as customer data, neither is it absolutely necessary for the operation of infrastructure. RPO for non mission-critical data - one full week or seven (7) days

## Versioning and retention

### Customer data
Will see a full backup once a week - every Thursday, 23:30 EEST, while incremental backups will happen every day at 23:30 EEST

Full backups will be stored for three (3) weeks, while only last seven (7) incremental backups will be retained.

In conclusion - Ten versions of data will be available, with better tuning available for the more recent data

### Grafana
Grafana config backups will be made every week at the same time as customer data backups - Thursday 23:30 EEST - each backup will be retained for two (2) weeks, leaving us with two versions of Grafana configuration

### Ansible repository
As the repository is powered by git, every version of the repository is going to be available with no (For the lifetime of the infrastructure) time constraint for storage

## Usability

To ensure the validity of data, on a machine separate from production, MySQL and Grafana will be randomly deployed and their functionality verified once a week

## Restoration criteria

Upon infrastructure operation error, first the correctness of configuration and operability of services should be insured, after which all the possible errors should go through troubleshooting

If and only if the abovementioned procedure won't yield a working infrastructure, restoration from backups can take place.

## Backup Recovery Time Objective

During the restoration, the whole infrastructure can be expected to be redeployed in one hour, while data backup restoration can be expected to take up to one additional hour

## Install and configure infrastructure with Ansible:

### Install with one play

    ansible-playbook infra.yaml
    Use this command if you dont know what is the issue!

    If you know the issue is with the backup, it is mandatory to first run the backup step (mentioned on the bottom) and only after you should run the play!

### Install/Restore with tags

    Keeping the order is necessary!

    ansible-playbook infra-yaml --tags init
    It will create a backup and update cache

    ansible-playbook infra-yaml --tags webserver
    It will run the plays that are required for the webserver to run

    ansible-playbook infra-yaml --tags exporter
    It will run the plays in order to exporters work

    ansible-playbook infra-yaml --tags grafana
    It will run all the necessary plays in order to grafana and its provisioning to work

    + ONLY DNS can be fixed with the following play, but it is included to webserver as well:
    ansible-playbook infra-yaml --tags bind

    + You can restore on database servers with the following command:
    ansible-playbook infra-yaml --tags db


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
