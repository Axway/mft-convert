{
    "title":"Data storage for high availability",
    "linkTitle":"Data storage for HA",
    "weight":"3",
    "date":"2019-08-09",
    "description":"Understand where API Portal stores API Management data."
}

## Data storage

API Portal stores API Management data both in relational database management system (RDBMS) and as files on disk. The stored data includes, for example, configuration data and logs, all API Portal customizations, as well as articles and their related data posted in Joomla!, blog, or discussion forum.

The following describes where API Portal stores data. In RDBMS, `#_` stands for the table prefix of the DB schema. The default `INSTALL_DIR` is `/opt/axway/apiportal/htdoc`.

### System

1. API Manager settings (For example, IP address, certificates.)

    * Files on disk
        * `INSTALL_DIR/administrator/components/com_apiportal/assets/cert`
    * RDBMS
        * `#_apiportal_configuration`

2. Logs

    * Files on disk
        * `INSTALL_DIR/logs`
        * (Software installation) `/var/log/httpd/error_log`

3. Public API mode key file

    * Files on disk
        * The custom path you specified for the encryption key at install time.

### Customization

1. Theme Magic customizations

    * Files on disk
        * `INSTALL_DIR/templates/purity_iii`

2. Stylesheets (CSS and LESS)

    * Files on disk
        * `INSTALL_DIR/templates/purity_iii/`

3. Company logo

    * Files on disk
        * `INSTALL_DIR/components/com_apiportal/assets/img/menu/`
    * RDBMS
        * `#_menu`

4. Menu entries and order

    * RDBMS
        * `#_menu`
        * `#_menu_types`

5. Changes in the PHP code

    * Files on disk
        * `INSTALL_DIR/components/com_apiportal/views/`

6. ReCaptcha plug-in

    * RDBMS
        * `#_extensions`

### Localization

1. Installed languages

    * Files on disk
        * `INSTALL_DIR/language`
        * `INSTALL_DIR/administrator/language`
        * `INSTALL_DIR/administrator/manifests/packages`
    * RDBMS
        * `#_extensions`

2. Language content

    * RDBMS
        * `#_menu`
        * `#_content`
        * `#_categories`

### Articles and posts

1. Joomla! articles

    * RDBMS
        * `#_content`
        * `#_content_frontpage`
        * `#_content_rating`
        * `#_content_types`
        * `#_content_item_tag_map`

2. Attachments uploaded to Joomla! content

    * Files on disk
        * `INSTALL_DIR/images`

3. Joomla! categories

    * RDBMS
        * `#_categories`

4. EasyBlog and EasyDiscuss settings and posts

    * RDBMS: Settings and Posts
        * `#_easyblog_configs`
        * `#_discuss_configs`

5. EasyBlog and EasyDiscuss attachments

    * Files on disk
        * `INSTALL_DIR/images/easyblog_*`
        * `INSTALL_DIR/media/com_easydiscuss`
    * RDBMS
        * `#_easyblog_configs`
        * `#_discuss_configs`

### Recommended API Portal data replication

API Portal data between API Portal instances or datacenters is replicated either automatically or manually.

Data can be replicated automatically using the following:

* **RDBMS cluster**: The configured database cluster replicates data between all database nodes in the cluster. For a technical example, see this article about [database cluster setup](https://support.axway.com/en/articles/article-details/id/180417) on Axway Support.
* **Shared file system solution**: A shared storage solution, like a cluster or shared network file system, synchronizes data across all instances. For a technical example, see this article about [shared file system](https://support.axway.com/en/articles/article-details/id/180405) on Axway Support.

In the manual, or static data replication, you must use a promotion tool to promote data from one API Portal instance to another. For details, see this article about the [promotion tool](https://support.axway.com/en/articles/article-details/id/180277) on Axway Support.

It is recommended to use automatic data replication whenever possible, and to configure RDBMS cluster for the databases and shared file system.

The replication options depend on the data type and where it is stored, as shown in the following table:

|Data type|Replication between datacenters|
|---------|----------------|
|**System**|   |
|API Manager settings (IP, cert)|Automatic, RDBMS cluster.|
|Logs|Not applicable (local file-based data only).|
|**Customization**|   |
|Theme Magic changes, stylesheets, company logo, changes in the PHP code.|Automatic (shared file system and RDBMS cluster) or static process (deployment process).|
|Menu entries and order, reCaptcha plug-in.|Automatic (RDBMS cluster) or static process (deployment process).|
|**Localization**|   |
|Languages Installed|Automatic (shared file system and RDBMS cluster) or static process (deployment process).|
|Languages Content|Automatic (RDBMS cluster) or static process (deployment process).|
|**Articles and posts**|   |
|Joomla! articles and categories, EasyBlog and EasyDiscussions settings and posts.|Automatic, RDBMS cluster.|
|Attachments uploaded to articles, blog posts, or discussion forum.|Automatic, shared file system.|
