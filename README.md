# IBM Security Guardium Analyzer Workshop

## Overview

Getting started with [IBM Security Guardium Analyzer](http://ibm.biz/analyzer) for efficiently finding regulated data, understand data and database exposures, and act to address issues and minimize risk.

Refer to slides at: [CASCON 2018 - Hands-On IBM Security GuardiumAnalyzer Bootcamp.pdf]()

**Table of Contents**

- [Getting Started](#getting-started)
  - [Register for Free Trial](#register-for-free-trial)
  - [Create IBM ID](#create-ibm-id)
  - [Setup Database](#setup-database)
    - [Databases Overview](#datababse-overview)
    - [Supported Databases](#supported-databases)
    - [Setup DB2 Database](#setup-db2-database)
- [First Time Login to IBM Security Guardium Analyzer](#first-time-login-to-ibm-security-guardium-analyzer)
  - [Setup Insight Settings](#setup-insight-settings)
  - [Download IBM Security Guardium Data Connector](#download-ibm-security-guardium-data-connector)
- [Setup IBM Security Guardium Data Connector](#setup-ibm-security-guardium-data-connector)
    - [Install Data Connector](#install-data-connector)
    - [Launch Data Connector](#launch-data-connector)
    - [Register Data Connector](#register-data-connector)
    - [Setup Default Database Location](#setup-default-database-location)
- [Add First Database to Data Connector](#add-first-databbase-to-data-connector)
  - [Add Database Connection Details](#add-database-connection-details)
  - [Add Database Scan Preference](#add-database-scan-preference)
  - [Start a Scan](#start-a-scan)
  - [View and manage Databases](#view-and-manage-databases)
- [View Results of Scan on Guardium Analyzer](#view-results-of-scan-on-guardium-analyzer)
  - [View Dashbaord Insights](#view-dashboard-insights)
  - [View Database Vulnerabilities](#view-dashboard-Vulnerabilities)
  - [View Database Patterns](#view-dashboard-Vulnerabilities)
  - [View Registered Data Connectors](#view-registered-data-connector)
  - [View Settings](#view-registered-data-connector)
    - [Notifications](#notifications)
    - [Patterns](#patterns)
    - [Scan Frequency](#scan-frequency)
    - [Manage Users](#manage-users)
- [Resources](#resources)

## Getting Started

### Register for Free Trial

1. Navigate to [Guardium Analyzer marketplace page](http://ibm.biz/analyzer). (Link: http://ibm.biz/analyzer) and click <img src="images/StartYourFreeTialButton.png" width="100" alt="Start your free tiral"> button.

<img src="images/AnalyzerMarketingPage.png" width="700" alt="Start your free tiral">

2. After clicking `Start your free trial` you will be navigated to either login with IBM ID or Create a new IBM ID. 

If you do not have an IBM ID follow the steps [Create IBM ID](#create-ibm-id).

If you do have an IBM ID login using your IBM ID. Then continue to [Ready To Use - IBM Security Guardium Analyzer Trial](#ready-to-use-ibm-security-guardium-analyzer-trial)

### Create IBM ID

1. Fill out form presented from step [Register for Free Trial](#register-for-free-trial) in the case you have not already created an IBM ID.

<img src="images/CreateIBMIDForm.png" width="700" alt="Start your free tiral">

2. Follow the additional steps as part of IBM ID creation including verifying email. 

<img src="images/CreateIBMIDVerify.png" width="700" alt="Start your free tiral">

3. Continue to step [Ready To Use - IBM Security Guardium Analyzer Trial](#ready-to-use-ibm-security-guardium-analyzer-trial)

### Ready To Use - IBM Security Guardium Analyzer Trial

1. Once you have successfully registered for `IBM Security Guardium Analyzer Trial` either through the process of `Create IBM ID` or `Login to existing IBM ID` you will receive an email that the service is ready to use.

<img src="images/ReadyToUseEmail.png" width="700" alt="Start your free tiral">

2. Click the <img src="images/LaunchService.png" width="100" alt="Start your free tiral"> link.

3. This will launch the `IBM Security Guardium Analyzer` Web UI.

Keep this link handy we will get back to this at [First Time Login to IBM Security Guardium Analyzer](#first-time-login-to-ibm-security-guardium-analyzer) step.

### Setup Database

`IBM Security Guardium Analyzer` helps you efficiently identify risk associated with personal and sensitive personal data (PII, PHI, PCI, etc.) that falls under regulations such as the E.U.’s GDPR, PCI DSS, HIPAA and other privacy mandates. The service applies next-generation data classification, as well as vulnerability scanning, to uncover privacy risks associated with such data in cloud-based and on-prem databases.

#### Supported Databases

Guardium Analyzer v1.0 scans on-premises and cloud databases including db2, Oracle, and
Microsoft SQL Server. 

Guardium Analyzer can scan databases that are either installed on an Infrastructure-as-a-Service (IaaS) solution (a cloud VM, for example) or hosted by a cloud provider as long as they are still db2, Oracle or SQL Server.

AWS RDS Oracle is fully tested, and we are testing Azure-MSSQL next.

#### Setup DB2 Database

Note: If you already have a data you would like to scan you can skip the step to setup a DB2 Database and use your own database on-prem or cloud based on [Supported Databases](#supported-databases)

For this workshop we will walkthrough how we can setup a DB2 Database and bootstrap the datababse with `db2sample`

##### Prerequisites

1. Install Docker: [Docker](https://www.docker.com/community-edition)
2. A Ubuntu 16.04.5 LTS physical or VM image (this what will use for this workshop)

##### Pull down DB2 Express C

```bash
$ docker run -it -p 50000:50000 -e DB2INST1_PASSWORD=db2inst1-pwd -e LICENSE=accept   -v  $(pwd):/share  ibmcom/db2express-c:latest bash
```

-p 50000:50000 exposes port 50000 to allow connections from the remote client.

By specifying -e DB2INST1_PASSWORD=db2inst1-pwd parameter, you set a password of your choice for the db2inst1 user for the default DB2 instance.

By specifying -e LICENSE=accept parameter, you are accepting this License to use the software contained in this image.

/share, referring to mount point at "/share" in the Docker.

$(pwd), the current directory on Docker host while running Docker command, which is mounted by Docker container. It can also be any existing directory on Docker host, like /tmp, /opt, etc.

##### Start DB2 and create sample DB

Note: You will be running these with in the DB2 Express C docker container

1. Login to `db2inst1` account

```bash
$ su - db2inst1
```

2. Start DB2

```bash
$ db2start
```

Should see `SQL1063N  DB2START processing was successful.` on a succesful DB2 Start.

3. Create a DB2 Sample Database

```bash
$ db2sampl
```

Should see the following on a successful DB2 Sample Database creation:

```

  Creating database "SAMPLE"...
  Connecting to database "SAMPLE"...
  Creating tables and data in schema "DB2INST1"...
  Creating tables with XML columns and XML data in schema "DB2INST1"...

  'db2sampl' processing complete.

```

4. Connect to DB2 Sample Database

```bash
$ db2 connect to sample
```

On a successful connection you should see: 

```
   Database Connection Information

 Database server        = DB2/LINUXX8664 10.5.5
 SQL authorization ID   = DB2INST1
 Local database alias   = SAMPLE
```

Note: Keep this terminal active as this will keep DB2 active and exposed to the network to access it. We will scan this database using the `IBM Security Data Connector`.

## First Time Login to IBM Security Guardium Analyzer

### Setup Insights Settings

### Download IBM Security Guardium Data Connector

## Setup IBM Security Guardium Data Connector

### Install Data Connector

### Launch Data Connector

### Register Data Connector

### Setup Default Database Location

## Add First Database to Data Connector

### Add Database Connection Details

### Add Database Scan Preference

### Start a Scan

### View and manage Databases

## View Results of Scan on Guardium Analyzer


### View Dashbaord Insights

### View Database Vulnerabilities

### View Database Patterns

### View Registered Data Connectors

### View Settings

#### Notifications

#### Patterns

#### Scan Frequency

#### Manage Users

## Resources