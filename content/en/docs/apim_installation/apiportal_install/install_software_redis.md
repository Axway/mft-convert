{
"title": "Configure Redis cache",
  "linkTitle": "Configure Redis",
  "weight": "50",
  "date": "2019-08-09",
  "description": "Configure a Redis cache to improve the performance of your API catalog."
}

For better performance and scalability, you can configure API Portal to cache APIs in a Redis cache. Using a Redis cache is recommended if you plan to expose hundreds of APIs, or you plan to connect API Portal to more than one API Manager.

{{< alert title="Note" color="primary" >}}While Redis support only caching of the API catalog page, [Elasticsearch](/docs/apim_installation/apiportal_install/install_software_elastic/) also supports Applications page caching. Therefore, we recommend using [Elasticsearch](/docs/apim_installation/apiportal_install/install_software_elastic/) instead of Redis.{{< /alert >}}

## Prerequisites

Before you configure API Portal to cache APIs in a Redis cache you must install at least one of the following Redis options:

* Install Redis server.
* Install Redis PHP extension on Red Hat 7 (RHEL 7) using RHSCL.
* Install Redis PHP extension on CentOS 7 or RHEL 8 using EPEL/Remi.

### Install Redis server

To install Redis server, download a Redis 5 or 6 server from [Redis.io](https://redis.io/download).

### Install Redis PHP extension on Red Hat 7 (RHEL 7) using RHSCL

Follow these steps to install a Redis PHP extension on RHEL 7:

1. Enable `rhel-server-rhscl-7-rpms` and `rhel-7-server-optional-rpms` repositories from RHSCL:

   ```shell
   sudo subscription-manager repos --enable rhel-server-rhscl-7-rpms
   sudo subscription-manager repos --enable rhel-7-server-optional-rpms
   sudo yum clean all
   ```

2. Install `rh-php73-php-devel` and `rh-php73-php-pear` to enable `pecl` (which manages external PHP modules) and `phpize` (which will build `redis` PHP module)`:

   ```shell
   sudo yum install rh-php73-php-devel rh-php73-php-pear
   sudo ln -s $(which pecl) /usr/bin
   ```

3. Install and enable `redis` PHP module:

   ```shell
   sudo pecl install redis
   echo "extension=redis.so" | sudo tee -a /etc/opt/rh/rh-php73/php.d/30-redis.ini
   ```

4. Verify `redis` PHP module was successfully enabled:

   ```shell
   php -m | grep redis
   ```

5. Restart Apache:

   ```shell
   sudo systemctl restart httpd24-httpd
   ```

6. (Optional) Remove `rh-php73-php-devel` and `rh-php73-php-pear`:

    ```shell
    sudo yum remove rh-php73-php-devel rh-php73-php-pear
    ```

### Install Redis PHP extension on CentOS 7 or RHEL 8 using EPEL/Remi

Follow these steps to install a Redis PHP extension on CentOS 7 or RHEL 8:

1. Install `redis5` PHP module:

   ```shell
   sudo yum install php-pecl-redis5
   ```

2. Verify `redis` PHP module is enabled:
   ```shell
   php -m | grep redis
   ```

3. Restart Apache:
   ```shell
   sudo systemctl restart httpd
   ```

## Enable Redis in API Portal

After you have successfully installed Redis, you must enable it in JAI:

1. In the Joomla! Administrator Interface (JAI), click **System > Global Configuration > System**.
2. On the **Cache configuration** section, set **Cache Handler** to **Redis**.
3. Review and update the other settings if necessary.
4. Click **Save**.

## Additional cache settings in API Portal

To change how long data is preserved in the cache:

1. In the JAI, click **Components > API Portal > Additional Settings**.
2. In **Cache Timeout**, enter how long (in seconds) APIs are preserved in the cache.
3. Click **Save**.

Use the **Purge cache** button to clear the cache at any time.

## Verify Redis installation

To verify that Redis is being used by API Portal, refresh the API Catalog page and enter the following command:

```
redis-cli keys *_apicatalog
```

## Further Redis configuration

You have learned how to install a basic Redis server, with no authentication or replication. For more advanced configuration, see [Redis documentation](https://redis.io/documentation).
