[ Backup SLA ]
/*\|/*\/*\|/*\/*\|/*\/*\|/*\/*\|/*\/*\|/*\/*\|/*\/*\|/*\|/*\
-[0] Introduction
############################################################
#-A service-level agreement is a commitment between a      #
#-service provider and a client. Particular aspects of     #
#-the service – quality, availability, responsibilities    #
#-are agreed between the service provider and the          #
#-service user.                                            #
############################################################
-[1] Backup objectives
#################################################################
#-Backing up the services is crucial part of the strong company.#
#-We provide best practices to backup services such as          #
#- (*) Database servers [InfluxDB][MySQL]                       #
#- (*) Ansible repository                                       #
#################################################################
-[2] Backup coverage -- (what is backed up and what is not)
####################################################################
#-Our main focus is databases, because they contain important data.#
#-Monitoring services, dns, web app and web server is not our      #
#-perspective.                                                     #
####################################################################
-[3] RPO -- (recovery point objective)
###################################################################
#-Due to high cost, our backup system is incremental. Incremental #   
#-backup system, restores data before the point where the loss has#
#-happened.                                                       #
###################################################################
-[4] Versioning and retention -- (how many backup versions are stored and for how long)
######################################################################
#-As it is said, incremental backup system has possibility to restore#
#-the data fully before the "loss point". Older intervals are stored #
#-for 1 month.                                                       #
######################################################################
-[5] Usability checks -- (how is backup usability verified)
#####################################################################
#-We create daily test database backup on your server. The data     #
#-is encrypted and sent over secure channel to our data center.     #
#-You have possibility to restore the data for checks and usability.#
#####################################################################
-[6] Restoration criteria -- (when should backup be restored)
####################################################
#-Restoring the data is first step when hardware or#
#-software failure happens and system shutsdown.   #
####################################################
-[7] RTO -- (recovery time objective)
###########################################################
#-RTO is maximum time objective that company is capable   #
#-of restoring the data. The RTO is not quite specified   #
#-and is more or less approximate. The reason we only     #
#-restore databases and ansible repository it requires    #
#-from 4-5 hours or more if we are dealing with giant data#
###########################################################
