{
"title": "Install Redis cache",
  "linkTitle": "Install Redis",
  "weight": "50",
  "date": "2019-08-09",
  "description": "Optionally install a Redis cache for better performance."
}
For better performance and scalability, you can configure API Portal to cache APIs in a Redis cache. Using a Redis cache is recommended if you plan to expose hundreds of APIs, or you plan to connect API Portal to more than one API Manager.

## Install Redis on Red Hat 7 (RHEL 7) using RHSCL

1. Enable `rhel-server-rhscl-7-rpms` and `rhel-7-server-optional-rpms` repositories from RHSCL:

   ```shell
   sudo subscription-manager repos --enable rhel-server-rhscl-7-rpms
   sudo subscription-manager repos --enable rhel-7-server-optional-rpms
   sudo yum clean all
   ```

2. Install Redis:

   ```shell
   sudo yum install rh-redis5
   ```

3. Enable Redis executables in any bash session by default:

   ```shell
   echo "source scl_source enable rh-redis5" | sudo tee -a /etc/profile.d/scl-redis5.sh
   source /etc/profile.d/scl-redis5.sh
   ```

4. Enable and start the Redis service:

   ```shell
   sudo systemctl enable --now rh-redis5-redis
   ```

5. Verify the Redis service is active:

   ```shell
   systemctl status rh-redis5-redis
   ```

6. Install `rh-php73-php-devel` and `rh-php73-php-pear` to enable `pecl` (which manages external PHP modules) and `phpize` (which will build `redis` PHP module)`:

   ```shell
   sudo yum install rh-php73-php-devel rh-php73-php-pear
   sudo ln -s $(which pecl) /usr/bin
   ```

7. Install and enable `redis` PHP module:

   ```shell
   sudo pecl install redis
   echo "extension=redis.so" | sudo tee -a /etc/opt/rh/rh-php73/php.d/30-redis.ini
   ```

8. Verify `redis` PHP module was successfully enabled:

   ```shell
   php -m | grep redis
   ```

9. Restart Apache:

   ```shell
   sudo systemctl restart httpd24-httpd
   ```

10. (Optional) Remove `rh-php73-php-devel` and `rh-php73-php-pear`:

    ```shell
    sudo yum remove rh-php73-php-devel rh-php73-php-pear
    ```

## Install Redis on CentOS 7 and RHEL 8 using EPEL/Remi

1. Install Redis and `redis` PHP module:

   ```shell
   sudo yum install redis php-pecl-redis5
   ```
2. Enable and start the Redis service:
   ```shell
   sudo systemctl enable --now redis
   ```
3. Verify the Redis service is active:
   ```shell
   systemctl status redis
   ```
4. Verify `redis` PHP module is enabled:
   ```shell
   php -m | grep redis
   ```
5. Restart Apache:
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
