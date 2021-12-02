[ Backup SLA ]<br>
/*\|/*\/*\|/*\/*\|/*\/*\|/*\/*\|/*\/*\|/*\/*\|/*\/*\|/*\|/*\<br>
-[0] Introduction<br>
############################################################<br>
#-A service-level agreement is a commitment between a      #<br>
#-service provider and a client. Particular aspects of     #<br>
#-the service – quality, availability, responsibilities    #<br>
#-are agreed between the service provider and the          #<br>
#-service user.                                            #<br>
############################################################<br>
-[1] Backup objectives<br>
#################################################################<br>
#-Backing up the services is crucial part of the strong company.#<br>
#-We provide best practices to backup services such as          #<br>
#- (*) Database servers [InfluxDB][MySQL]                       #<br>
#- (*) Ansible repository                                       #<br>
#################################################################<br>
-[2] Backup coverage -- (what is backed up and what is not)<br>
####################################################################<br>
#-Our main focus is databases, because they contain important data.#<br>
#-Monitoring services, dns, web app and web server is not our      #<br>
#-perspective.                                                     #<br>
####################################################################<br>
-[3] RPO -- (recovery point objective)<br>
###################################################################<br>
#-Due to high cost, our backup system is incremental. Incremental #<br>   
#-backup system, restores data before the point where the loss has#<br>
#-happened.                                                       #<br>
###################################################################<br>
-[4] Versioning and retention -- (how many backup versions are stored and for how long)<br>
######################################################################<br>
#-As it is said, incremental backup system has possibility to restore#<br>
#-the data fully before the "loss point". Older intervals are stored #<br>
#-for 1 month.                                                       #<br>
######################################################################<br>
-[5] Usability checks -- (how is backup usability verified)<br>
#####################################################################<br>
#-We create daily test database backup on your server. The data     #<br>
#-is encrypted and sent over secure channel to our data center.     #<br>
#-You have possibility to restore the data for checks and usability.#<br>
#####################################################################<br>
-[6] Restoration criteria -- (when should backup be restored)<br>
####################################################<br>
#-Restoring the data is first step when hardware or#<br>
#-software failure happens and system shutsdown.   #<br>
####################################################<br>
-[7] RTO -- (recovery time objective)<br>
###########################################################<br>
#-RTO is maximum time objective that company is capable   #<br>
#-of restoring the data. The RTO is not quite specified   #<br>
#-and is more or less approximate. The reason we only     #<br>
#-restore databases and ansible repository it requires    #<br>
#-from 4-5 hours or more if we are dealing with giant data#<br>
###########################################################<br>
