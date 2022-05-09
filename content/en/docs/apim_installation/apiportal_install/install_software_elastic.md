{
  "title": "Configure Elasticsearch",
  "linkTitle": "Configure Elasticsearch",
  "weight": "50",
  "date": "2021-08-23",
  "description": "Configure Elasticsearch to improve API and Applications management performance."
}

When you connect your API Portal to an Elasticsearch server, API Portal can fetch all the API and Applications data from API Manager instances and load it into the Elasticsearch datastore.

This connection also creates a pagination of 10 items per page in the API and Applications catalogs, which improves performance because only the configured number of resources per page are fetched and loaded into the server memory instead of the full list.

Integration with Elasticsearch allows you to achieve the same caching capabilities as [caching with Redis](/docs/apim_installation/apiportal_install/install_software_redis/), and in addition, pagination with much better performance. While Redis support only caching of the API catalog page, Elasticsearch  also supports Applications page caching. Therefore, we recommend using Elasticsearch instead of [Redis](/docs/apim_installation/apiportal_install/install_software_redis/).

## Prerequisites

* You must have an up and running Elasticsearch server.
* Connecting Elasticsearch to a single node in API Portal is enough.
* You must have `cron` and `crontab` installed, and a `crond` service running in your API Portal server. (You can usually get `cron` by default with RHEL and CentOS Linux.) This is not required for API Portal in a Docker container.

## Elasticsearch security practices

When setting up your Elasticsearch server, observe the following recommendations:

* Elasticsearch server is not externally facing. Only the needed applications, like API Portal and Kibana, should have access to it on dedicated ports.
* Enable Elasticsearch security features, and configure username and password for the built-in users. While Kibana is not required, we recommend it as an official client for ES.
* Configure a secure HTTPS traffic so that all the communication between API Portal and Elasticsearch is secured.

For more information, see the [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/security-minimal-setup.html) documentation.

## Add API Manager credentials to API Portal

API Portal needs the API Manager administrator user credentials to collect all APIs and Applications from API Manager and push them to the Elasticsearch.

Follow these steps to configure API Manager admin user credentials into your API Portal:

1. In the Joomla! Administrator Interface (JAI), click **Components > API Portal > API Manager**.
2. Enter API Manager admin user credentials under the **Credentials for APIadmin user** section. Repeat this step for all API Manager instances.
3. Click **Components > API Portal > Additional API Managers**.
4. Click **Save**.

## Connect API Portal with Elasticsearch

Follow these steps to connect API Portal to the Elasticsearch server, and to configure a scheduled indexation for APIs and Applications.

1. In JAI, click **Components > API Portal > Elasticsearch Settings**.
2. Choose "Yes" under **Enable Elasticsearch**.
3. Populate the connection data fields:
   * Protocol
   * Host
   * Port
   * Username
   * User Password
4. Click **Save**.

## Configure a schedule to push data to Elasticsearch

After connecting API to Elasticsearch, populate the crontab fields to create a schedule to push data to Elasticsearch.

1. In JAI, click **Components > API Portal > Elasticsearch Settings**.
2. Populate the following fields:
   * **APIs Cron Schedule**. This is to schedule Elasticsearch indexing for APIs from API Managers.
   * **Applications Cron Schedule**. This is to schedule Elasticsearch indexing for Applications from API Managers.
3. Click Save.

After that, APIs and Applications will automatically reindex based on the cron schedule setting.

### Considerations before creating a schedule

Before you configure your time schedule interval for indexing APIs and Applications, consider the following to help you to decide on what are the most appropriate schedule indexation intervals for your environment.

* How long it takes to push the data to the index?

This will depend on some aspects like the number of APIs and Applications, infrastructure resources (your server's calculating power), and network capabilities.

To discover this time, you can [trigger the indexation manually](#push-data-to-elasticsearch-manually) and monitor how long it takes for the process to end. You must perform this for APIs and Applications separately because they have different schedules.

* How often does your data in API Manager gets changed?

If your data changes frequently (adding, updating, or deleting APIs and Applications) we recommend that you use the minimum required time period (the value that you found when monitoring the process).

If your data does not change frequently, you can configure longer intervals for the scheduled indexation to run, for example, every night, or even once every three days or so.

## Push data to Elasticsearch manually

If you are using API Portal in containers, you can trigger APIs and Applications indexing manually.

After connecting API to Elasticsearch, in JAI, click **Components > API Portal > Elasticsearch Settings**, then click **Trigger APIs Indexing** and **Trigger Applications Indexing** to push data to Elasticsearch.
