Type: Blog Post (Markdown)
Blog: blog.4aiur.net
Link: http://blog.4aiur.net/2012/05/openshift-command-line-interface/
Post: 902
Title: OpenShift Command Line Interface
Slug: openshift-command-line-interface
Postformat: standard
Keywords: OpenShift
Status: publish
Date: 2012-05-25 10:02:57 +0800
Pings: On
Comments: On
Category: Python

OpenShift Command Line Interface
=======

1. Working With Domains

    1. Creating a Domain
    
            rhc domain create -n DomainName -l rhlogin -p password [OptionalParameters]
    
    2. Deleting a Domain
    
            rhc domain show -l rhlogin
            rhc app destroy -a <applicationName> -l rhlogin
            rhc domain destroy -n <domainName> -l rhlogin

2. Viewing User Information

        rhc domain show -l rhloging

3. Creating Applications

    Supported app types:
    * nodejs-0.6
    * jbossas-7
    * python-2.6
    * jenkins-1.4
    * ruby-1.8
    * diy-0.1
    * php-5.3
    * perl-5.10

        rhc app create -a appName -t appType

    Using Arbitrary DNS Names

        rhc app add-alias -a racer --alias "racer.indy.com"
        rhc app remove-alias -a racer --alias "racer.indy.com"

4. Managing Applications

        rhc app <action> -a ApplicationName [Optional Arguments]

    Table Application management command argument options
    
    Option        | Details
    ------------ | -------------
    start         | Start an application in the cloud.
    stop          | Stop an application that is currently running in the cloud.
    restart   | Restart an application in the cloud.
    reload        | Reload an application in the cloud.
    status        | Display the current status of an application in the cloud.
    destroy   | Remove an application from the cloud.

5. Using Cartridges

        rhc-ctl-app -L

    **Database Cartridges**

    * MySQL Database 5.1 — MySQL is a multi-user, multi-threaded SQL database server
    * MongoDB NoSQL Database 2.0 — MongoDB is a scalable, high-performance, open source NoSQL database
    * PostgreSQL Database 8.4 — PostgreSQL is an advanced Object-Relational database management system

    **Management Cartridges**

    * phpMyAdmin 3.4 — phpMyAdmin is a web-based MySQL administration tool
    * RockMongo 1.1 — RockMongo is a web-based MongoDB administration tool
    * Jenkins Client 1.4 — a client for managing Jenkins-enabled applications
    * Cron 1.4 — Cron is a daemon that runs specified programs at scheduled times
    * OpenShift Metrics 0.1 — OpenShift Metrics is an experimental cartridge for monitoring applications

6. Managing Applications in a Shell Environment

    **Viewing Application Information**

        $ rhc domain show
        Password:
        Application Info
        ================
        racer
            Framework: php-5.3
             Creation: 2011-12-07T01:07:33-05:00
                 UUID: 0bd9d81bfe0a4def9de47b89fe1d3543
              Git URL: ssh://0bd9d81bfe0a4def9de47b89fe1d3543@racer-ecs.rhcloud.com/~/git/racer.git/
           Public URL: http://racer-ecs.rhcloud.com/
        
    **Opening a Shell Environment for an Application Node**

        $ ssh 0bd9d81bfe0a4def9de47b89fe1d3543@racer-ecs.rhcloud.com
        Warning: Permanently added 'racer-ecs.rhcloud.com,174.129.154.20' (RSA) to the list of known hosts.
        
            Welcome to OpenShift shell
        
            This shell will assist you in managing openshift applications.
        
            type "help" for more info.
        
        [racer-ecs.rhcloud.com ~]\>
