---
title: Overview of API Gateway Analytics
linkTitle: Overview
weight: 1
date: 2019-10-03
description: Gives a brief overview of API Gateway Analytics and outlines the steps required to set up and use it.
---
API Gateway Analytics is a server runtime and web-based monitoring and reporting console that enables you to generate scheduled reports and analyze API use over time in multiple API Gateways across the domain.

![API Gateway Analytics](/Images/concepts/reporter.png)

API Gateway Analytics includes the following features:

* Web-based console that monitors and reports on all API Gateways in the domain (multiple API Gateways are shown on the left in the diagram)
* Reporting over an extended time period rather than immediate operational monitoring
* Analysis of what APIs are used, how often APIs are used, when APIs are used, and who is using APIs
* Scheduled reports in PDF format can be emailed to specific users

## Install API Gateway Analytics

This guide assumes that you have already installed API Gateway Analytics. For more details, see the
[Install API Gateway Analytics](/docs/apim_installation/apigtw_install/install_analytics/).

For details on upgrading API Gateway Analytics from earlier versions, see the
[Upgrade API Gateway Analytics](/docs/apim_installation/apigw_upgrade/upgrade_analytics/).

## Configure your API Gateway Analytics metrics database

Before starting API Gateway Analytics, you must perform the following steps:

1. Create the third-party JDBC database instance used to store metrics. For more details, see [Install and configure a metrics database](/docs/apim_installation/apigtw_install/metrics_db_install/). Alternatively, if you already have an existing database, skip to the next step.
2. Update your API Gateway Analytics configuration with the database details using the `configureserver` script. For more details, see [Configure API Gateway Analytics](/docs/apimanager_analytics/analytics_config).
3. Configure the database tables using the `dbsetup` script. For more details, see [Install and configure a metrics database](/docs/apim_installation/apigtw_install/metrics_db_install/).
4. Configure your API Gateway instance and Admin Node Manager to store metrics. For more details, see [Configure API Gateway with the metrics database](/docs/apimanager_analytics/metrics_gw_config).

For more details on managing metrics, see [Purge the metrics database](/docs/apimanager_analytics/metrics_db_purge).

## Monitoring and reporting with API Gateway Analytics

When you have configured the metrics database and API Gateway Analytics, you can start monitoring your API traffic and generating reports in API Gateway Analytics. For details, see [Get started with API Gateway Analytics] (/docs/apimanager_analytics/analytics_start/).
