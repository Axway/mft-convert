{
    "title":"Upgrade API Portal docker deployment",
    "linkTitle":"Upgrade docker deployment",
    "draft": "true",
    "weight":"5",
    "date":"2019-08-09",
    "description":"Upgrade your API Portal Docker deployment, preserving any API Portal customizations."
}

This topic describes how to upgrade an API Portal Docker deployment. The upgrade preserves any API Portal customizations (for example, new menus, new templates, localizations, and so on).

Upgrade to API Portal 7.7 is supported from API Portal 7.6.2 only. To upgrade from earlier versions, you must first upgrade to 7.6.2.

{{< alert title="Caution" color="warning" >}}

* Before you upgrade you must back up the API Portal container and the database container.
* Do not modify the content of the following folders, as they will be overwritten during upgrade.
    * `INSTALL_DIR/templates/purity_iii`
    * `INSTALL_DIR/language/en-GB`
    * `INSTALL_DIR/language/overrides`
    * `INSTALL_DIR/administrator/language/en-GB`
    * `INSTALL_DIR/language/overrides`
{{< /alert >}}

## Upgrade steps

Follow the steps in this section to upgrade your API Portal Docker deployment.

### Create Docker data volumes for your API Portal customizations

In this section you will:

* Use the `docker commit` command to create a new Docker image from your old version API Portal image.
* Create Docker data volumes to store the customizations from your old version API Portal.
* Run a new Docker container from the committed image to force Docker to copy the customizations to the Docker data volumes you created.

Perform the following steps:

1. To create a new Docker image containing your customized API Portal, execute the following command on your running old version API Portal container:

    `$ docker commit <old API Portal container> <old API Portal image containing customizations>`

2. If Public API mode is enabled, copy the encryption key file to your host machine:

    `$ docker cp <old API Portal container>:<path to encryption key in container>  <path on host machine>`

3. Stop the old version API Portal container:

    `$ docker stop <old API Portal container>`

4. Create data volumes for all the API Portal customized data using the following commands:

    `$ docker volume create template`

    `$ docker volume create images`

5. Run the container from the newly committed API Portal Docker image with the created data volumes pointing to the folders with customized files using the following command:

    ```
    docker run -d --name <customized old API Portal container> -e container=docker -e answer=y -e mysqlSSLModeAnswer=n -e mysqlHost=<IP of your MySQL container> -e mysqlPort=<MySQL port> -e mysqlUsername=root -e mysqlPassword=<root password> -e mysqlDbName=<MySQL database name> -p <host machine port>:<API Portal port> -v templates:/opt/axway/apiportal/htdoc/templates -v images:/opt/axway/apiportal/htdoc/images <old API Portal image containing customizations>
    ```

6. Verify that API Portal is functional and all the customizations are preserved.
7. Stop the customized API Portal container using the command:

    `$ docker stop <customized old API Portal container>`

### Upgrade the database

This section is only required if you want to upgrade the database itself. If you do not want to upgrade the database you can skip this section.

To upgrade your database, follow these steps:

1. To inspect the current configuration of your database container, use the following command:

    `$ docker inspect <database container>`

    {{< alert title="Note" color="primary" >}}Look for `Mounts[]` settings in the output. All database containers use volumes to store the data (regardless of which image was used to start the container). This volume is persisted on the host machine and is used when deploying the new version of the API Portal or database container.{{< /alert >}}

    Example output:

    ```
    "Mounts": [
    {
    "Type": "volume",
    "Name": "39b39d02ce1f046d44231e12757cb2d12062420cc29f200502d3444bf4338c73",
    "Source": "/mnt/sda1/var/lib/docker/volumes/39b39d02ce1f046d44231e12757cb2d12062420cc29f200502d3444bf4338c73/_data",
    "Destination": "/var/lib/mysql/data",
    "Driver": "local",
    "Mode": "z",
    "RW": true,
    "Propagation": ""
    }\
    ]
    ```

2. To stop the current database container, use the following command:

    `$ docker stop <database container>`

3. Download the required version of the database container from the [official Docker Hub repository](https://hub.docker.com/). For example:

    * To download the latest `MySQL` image, execute the following command: `$ docker pull mysql`

    * To download the latest `centos/mysql-57-centos7` image, execute the following command: `$ docker pull centos/mysql-57-centos7`

    * To download the latest `MariaDB` image, execute the following command: `$ docker pull mariadb`

4. To start the database container from the downloaded image and specify the data volume from the previous database container, use the following command:

    ```
    docker run -d --name <new database container> -e MYSQL_ROOT_PASSWORD=<your root password> -p <host machine port>:<MySQL port> -v <volume name>:<volume destination> <imagename>:<version>`
    ```

5. To inspect the new database container and find the IP address of the container, use this command:

    `$ docker inspect <new database container>`

6. To connect to the container, use the following command:

    `$ docker exec -it <new database container> bash`

7. To verify that the database schema exists, you can use the following commands:

    `$ mysql -u root -p <MYSQL_ROOT_PASSWORD>`

    `$ show databases;`

### Download and run API Portal 7.7 container

To download and run an API Portal 7.7 Docker container with the customizations from your old API Portal version, follow these steps:

1. Download the sample Docker package for API Portal 7.7 (for example, `APIPortal_7.7_SamplesPackageDocker_linux-x86-64_BN<build number>.zip`) from Axway Support at [https://support.axway.com](https://support.axway.com/).
2. Upload the package to your Docker host machine.
3. Unzip the package.
4. Ensure that you are logged in to your Docker host machine as the `root` user.
5. Change to the `apiportal` directory within the unzipped sample package, and enter the following command to build the API Portal image:

    `$ docker build -t <imagename>:<version>`

    For example:

    `$ docker build -t apiportal:7.7`

6. If Public API mode is enabled, specify a volume for it:

    `$ docker volume create <encryption key volume>:/apiportal/encryption`

7. Run a container from the new API Portal 7.7 Docker image, and specify the data volumes you created in [Create Docker data volumes for your API Portal customizations](#Create):

    ```
    docker run -it --name <new API Portal container> -e MYSQL_HOST=<IP of your DB container> -e MYSQL_PORT=<DB port> -e MYSQL_ROOT_PASSWORD=<root password> -e MYSQL_USERNAME=<username> -e MYSQL_PASSWORD=<user password> -e MYSQL_DBNAME=<database name> -p <host machine port>:<API Portal port> -v templates:/opt/axway/apiportal/htdoc/templates -v images:/opt/axway/apiportal/htdoc/images <imagename>:<version>
    ```

8. This command runs an API Portal 7.7 Docker container with all of the customizations from your old API Portal version preserved.
9. If Public API mode is enabled, copy the encryption key file to the container:

    ```
    docker cp <path on host machine> <API Portal container name>:/apiportal/encryption/encryption.key
    ```

10. Check that the volumes contain proper data:

    ```
    docker volume inspect  <volume name > --format "{{ .Mountpoint }}" | ls -lA
    ```

API Portal 7.7 is now running in a Docker container fully configured and ready to use. To go to the API Portal landing page, enter the host address and the host port in a browser.

You must access API Portal with the `HOST_MACHINE_ADDRESS` and the `HOST_MACHINE_EXPOSED_PORT` (not the container address and port). For example, `https://<HOST_MACHINE_ADDRESS>:<HOST_MACHINE_EXPOSED_PORT>`.

## Post-upgrade steps

After the upgrade, perform the following tasks.

### Reinstall Joomla! components

After upgrade, you must reinstall Easyblog and EasyDiscuss in JAI to update the component version and fix compatibility issues. The API Portal data related to the components (posts, attachments) is not affected.

1. Log in to the JAI.
2. Click **Components > EasyBlog**, and follow the instructions in the EasyBlog installer.
3. If prompted to select the installation method, select **Installation via Directory**, select the available package from the drop-down list, and follow the instructions in the installer to the finish.

    Do not install any of the modules and plugins unless you plan to use them. To prevent installing any modules, click **Modules** and deselect **Select All**, then repeat the same for **Plugins**.
4. Click **Components > EasyDiscuss**, and repeat the component installation as described for EasyBlog.

{{< alert title="Note" color="primary" >}} To resolve a known issue (caused by EasyBlog) with broken menu paths when creating new custom menus for your API Portal in JAI, you must rebuild the menu paths. In JAI, select **Menus > Main Menu** and click **Rebuild**. You only need to rebuild the menu paths once after installation or upgrade. {{< /alert >}}

### Restore footer customizations

If you customized the company name in your API Portal footer using a Joomla! language override (**Extensions > Languages > Overrides** in JAI), you must perform the following steps to restore language overrides after upgrade:

1. Locate the backup file `/opt/axway/apiportal/htdoc/Backups/<timestamp>/administrator/language/overrides/en-GB.override.ini` that is created during an upgrade. The timestamp corresponds to the time of the upgrade, for example, `20180613105149`.
2. Copy the backup file to the path `/opt/axway/apiportal/htdoc/language/overrides/en-GB.override.ini`.

Any customizations performed using language overrides are now restored.

### Reconfigure API Portal container to use Redis cache

If you are using Redis cache, you must configure the new API Portal 7.7 container to use the Redis cache. Perform the following steps:

1. Check the IP address of the Redis container:

    `$ docker inspect <your Redis container name>`

2. Enter the following command to connect to the API Portal container:

    `$ docker exec -it <your API Portal container name> /bin/bash`

3. Open the following configuration file for editing:

    `/opt/axway/apiportal/htdoc/configuration.php`

4. Locate the following line:

    `redis_server_host = 'localhost';`

5. Change `localhost` to the IP address of the Redis container and save the file.

### Encrypt the Public API mode user password (optional)

If you are using the Public API mode in API Portal you must run a script to encrypt the Public API mode user password and specify a directory to store the encryption key. For details, see [Encrypt the Public API user password (optional)](/docs/apim_installation/apiportal_docker/docker_portal_deploy/#encrypt-the-public-api-user-password-optional).
