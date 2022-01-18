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

Acceptable time window of customer data loss is from monday to saturday.

In case of the other data source (Manually configured Grafana) - acceptable time window grows considerably, as it does not see changes nearly as often as customer data, neither is it absolutely necessary for the operation of infrastructure.

## Versioning and retention

### Customer data
Will see a full backup once a week - every Saturday, 23:30 EEST, while incremental backups will happen every day at 23:30 EEST
In conclusion - Ten versions of data will be available, with better tuning available for the more recent data

### Grafana
Grafana config backups will be made from monday to friday at the same time as customer data backups - Thursday 23:30 EEST - each backup will be retained for two (2) weeks, leaving us with two versions of Grafana configuration

### Ansible repository
As the repository is powered by git, every version of the repository is going to be available with no (For the lifetime of the infrastructure) time constraint for storage

## Usability

To ensure the validity of data, on a machine separate from production, MySQL and Grafana will be randomly deployed and their functionality verified once a week

## Restoration criteria

Upon infrastructure operation error, first the correctness of configuration and operability of services should be insured, after which all the possible errors should go through troubleshooting

If and only if the abovementioned procedure won't yield a working infrastructure, restoration from backups can take place.

## Backup Recovery Time Objective

During the restoration, the whole infrastructure can be expected to be redeployed in one hour, while data backup restoration can be expected to take up to one additional hour
