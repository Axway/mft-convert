{
"title": "Run API Portal using ready-made Docker image",
  "linkTitle": "Run using ready-made Docker image",
  "weight": "20",
  "date": "2019-08-09",
  "description": "Use the ready-made API Portal Docker image to run your API Portal in Docker containers."
}

The ready-made image is ready out-of-the-box, so you do not have to build it using the `Dockerfile`.

## Prerequisites

The following components are required on your system before you can deploy API Portal in Docker containers:

* [Docker engine](https://docs.docker.com/engine/).
* MySQL server.
* API Portal Docker image, available from [Axway Support](https://support.axway.com/en/search/index/type/Downloads/q/API%20Portal%20/ipp/10/product/545/version/3036/subtype/89).

Optional components:

* Redis server, used for API Catalog caching.
* ClamAV, used for scanning of uploaded files.
* Elasticsearch server, used for API Catalog and Applications list pages performance optimization.

The monitoring feature of API Portal, which enables your API consumers to monitor application and API usage, requires a connected API Manager with monitoring metrics enabled.

## Run a Docker container using the image

1. Download the API Portal Docker image from [Axway Support](https://support.axway.com/en/search/index/type/Downloads/q/API%20Portal%20/ipp/10/product/545/version/3036/subtype/89).
2. Upload the file to your Docker host machine.
3. Enter the following command to load the image:

   ```
   docker load -i APIPortal_7.7_Docker_Image_linux-x86-64_<build number>.tgz
   ```
4. Run the API Portal Docker container, for example:

   ```
   docker container run --name apiportal \
     -d -p 8080:80 \
     -e MYSQL_HOST=mysql.axway.com \
     -e MYSQL_PORT=3306 \
     -e MYSQL_DATABASE=joomla \
     -e MYSQL_USER=joomla \
     -e MYSQL_PASSWORD=XXXXX \
     apiportal:7.7
   ```

   This example performs the following:

   * Runs an API Portal Docker container from an image named `apiportal`:`7.7` in detached mode.
   * Sets environment variables for connecting to the MySQL server.
   * Binds port 80 of the container to port 8080 on the host machine.

API Portal is now running in a Docker container.

To access your API Portal, you must first link it to your API Manager. For more details, see [Connect API Portal to API Manager](/docs/apim_installation/apiportal_install/connect_to_apimgr/).

If you plan to configure API Manager with environment variables, you must [install API Manager and API Gateway](/docs/apim_installation/apigtw_install/) on-premise or in containers before you deploy API Portal in containers.

{{< alert title="Note" color="primary" >}}
API Portal Docker container exposes Apache ports `80` and `443`. Port `443` is used only when SSL in Apache is configured.
{{< /alert >}}

## Access API Portal Docker image self-documentation

To get help with the Docker image, run the following command:

```
docker container run --rm <apiportal-image-tag> --help
```

To list the environment variables available in the Docker image, run the following command:

```
docker container run --rm <apiportal-image-tag> --env
```

## Configure API Portal runtime with environment variables

API Portal container supports a wide range of environment variables that allows you to configure API Portal runtime and some of the Joomla! Administrator Interface (JAI) settings.

The following is an example that you can copy and paste to an `env` file, then edit the values and use the `env` file with `docker run` command:

```
##### NOTE #####
# Boolean environment variables can take a value of 0 or 1.
# `*_CONFIGURED` and `*_ON` variables are boolean.
################

##### REQUIRED SETTINGS #####
# This configuration settings are required
# for API Portal docker container to boot.
#############################
MYSQL_HOST=
MYSQL_PORT=3306
MYSQL_USER=
MYSQL_PASSWORD=
MYSQL_DATABASE=

##### OPTIONAL SETTINGS #####
# The rest of this configuration settings are optional.
#############################
# Certificates can be passed in plain text or as base64 encoded string.
# For base64 encoded values prepend them with `base64:`
#
# Example:
# APACHE_SSL_CERT=base64:<base64-encoded-certificate>
# APACHE_SSL_PRIVATE_KEY=<plain-text-private-key>
# MYSQL_SSL_CA_CERT=base64:<base64-encoded-certificate>
#############################
#
#####
# For reference see "Run API Portal with HTTPS"
# under "Install API Portal" page in API Portal docs
#
# API Portal docker image doesn't provide auto-generated
# self-signed certificate option
#####
APACHE_SSL_ON=0
APACHE_SSL_CERT=
APACHE_SSL_PRIVATE_KEY=

#####
# For reference see "Configure the database server for secure connection"
# under "Install and configure database server" page in API Portal docs
#
# Two-way (mutual) authentication will be configured only when all three
# certificates are provided. Otherwise, one-way (Server CA) authentication
# will be used
#
# `MYSQL_SSL_VERIFY_CERT` is boolean
#####
MYSQL_SSL_ON=0
MYSQL_SSL_CA_CERT=
MYSQL_SSL_CLIENT_CERT=
MYSQL_SSL_CLIENT_KEY=
MYSQL_SSL_VERIFY_CERT=1

##### CHANGEABLE SETTINGS #####
# The rest of configuration settings can be configured in JAI as well.
#
# `*_CONFIGURED` option determines whether the feature is configured with
# environment variables. For example `APIMANAGER_CONFIGURED=0` means
# that the rest of `APIMANAGER_*` variables won't effect the runtime.
# On the other hand, `APIMANAGER_CONFIGURED=1` will configure API Manager
# with values from environment variables.
#
# !!! If you set `*_CONFIGURED=1`, all the changes made in JAI will be
# overridden by values from environment variables on the container restart
#
# `*_ON` option determines whether the feature is enabled.
##############################
#
#####
# For reference see "Connect API Portal to a single API Manager"
# under "Connect API Portal to API Manager" page in API Portal docs
#####
APIMANAGER_CONFIGURED=0
APIMANAGER_NAME=Master
APIMANAGER_HOST=
APIMANAGER_PORT=8075

#####
# For reference see "Customize Try-it by type of request"
# under "Customize API Catalog" page in API Portal docs.
#
# All `TRYIT_METHODS_*` variables are boolean
#####
TRYIT_METHODS_CONFIGURED=0
TRYIT_METHODS_ENABLE_GET=1
TRYIT_METHODS_ENABLE_POST=1
TRYIT_METHODS_ENABLE_PUT=1
TRYIT_METHODS_ENABLE_DELETE=1
TRYIT_METHODS_ENABLE_PATCH=1
TRYIT_METHODS_ENABLE_HEAD=1
TRYIT_METHODS_ENABLE_OPTIONS=1

#####
# For reference see "Change the page displayed after first login"
# under "Additional customizations" page in API Portal docs.
#####
REDIRECT_AFTER_LOGIN_CONFIGURED=0
REDIRECT_AFTER_LOGIN_URL=

#####
# For reference see "Customize source of API descriptions"
# under "Customize API Catalog" page in API Portal docs.
#
# `API_INFORMATION_SOURCE_NAME` can take a value of `summary` or `description` (case sensitive)
#####
API_INFORMATION_SOURCE_CONFIGURED=0
API_INFORMATION_SOURCE_NAME=summary

#####
# `MONITORING_MONTH_RANGE_VALUE` is an integer, which range is 2 to 6
#####
MONITORING_MONTH_RANGE_CONFIGURED=0
MONITORING_MONTH_RANGE_VALUE=2

#####
# For reference see "Absolute session timeout"
# under "Additional customizations" page in API Portal docs.
#####
ABSOLUTE_SESSION_TIMEOUT_CONFIGURED=0
ABSOLUTE_SESSION_TIMEOUT_HOURS=24

#####
# For reference see "Enable scanning of uploaded files"
# section in "Secure API Portal" page in API Portal docs
#####
CLAMAV_CONFIGURED=0
CLAMAV_ON=0
CLAMAV_HOST=
CLAMAV_PORT=3310

#####
# For reference see "API Portal single sign-on"
# page in API Portal docs
#####
SSO_CONFIGURED=0
SSO_ON=0
SSO_PATH=
SSO_ENTITY_ID=
SSO_WHITELIST=

#####
# For reference see reCapcha related topics
# under "Additional customizations" page in API Portal docs
#
# `LOGIN_PROTECTION_LOCK_IP` is boolean
#####
LOGIN_PROTECTION_CONFIGURED=0
LOGIN_PROTECTION_ON=0
LOGIN_PROTECTION_ATTEMPTS_BEFORE_RECAPCHA=3
LOGIN_PROTECTION_ATTEMPTS_BEFORE_LOCK=3
LOGIN_PROTECTION_LOCK_DURATION_SEC=600
LOGIN_PROTECTION_LOCK_IP=0

#####
# For reference see "Secure API Portal"
# page in API Portal docs
#
# `OAUTH_WHITELIST` is a comma separated string
#####
OAUTH_WHITELIST_CONFIGURED=0
OAUTH_WHITELIST=

#####
# For reference see "Secure API Portal"
# page in API Portal docs
#
# `API_WHITELIST` is a comma separated string
#####
API_WHITELIST_CONFIGURED=0
API_WHITELIST=

#####
# JAI admin management.
# Works only at first run.
#####
# JAI admin account password.
# Leave blank for container default.
ADMIN_PASSWORD=
# Disable reset JAI admin password
# upon first login. Boolean value
ADMIN_PASSWORD_RESET_DISABLED=0

##### MISC SETTINGS #####
# Settings that don't fit any other section
###################################
# Specify a timezone to use.
# Leave blank for container default.
# Example: America/New_York
TZ=

##### NON PERSISTING SETTINGS #####
# Settings under this section don't persist if you configure
# it in JAI, they will be reset after container restart. So, in a common
# use case they should be configured via environment variables.
###################################
#
#####
# For reference see "Install Redis cache"
# page in API Portal docs
#####
REDIS_CONFIGURED=0
REDIS_ON=0
REDIS_HOST=
REDIS_PORT=6379
REDIS_CACHE_TIMEOUT_SEC=600
```

The following is an example of how to use the `env` file with `docker run` command:

```
docker container run --env-file .env \
  -e MYSQL_PASSWORD=very_secret_password \
  -e APACHE_SSL_PRIVATE_KEY="$(cat ~/certs/apiportal.key)" \
  <more-options...>
```

Note that inline environment variables take precedence over variables from the `env` file. So, in this example, `very_secret_password` and certificate, taken from `apiportal.key` file, will override the password and certificate in the `.env` file.

### Configure certificates using volumes

Besides configuring certificates with environment variables, alternatively you can use volumes. This type of configuration overrides values from previously set environment variables.

Certificate files are placed in the following locations inside the container:

```
/opt/axway/apiportal/certs/
├── apache/
│   ├── apache.crt
│   └── apache.key
└── mysql/
    ├── mysql-ca.pem
    ├── mysql-client-cert.pem
    └── mysql-client-key.pem
```

The following example uses `APACHE_SSL_ON` and `MYSQL_SSL_ON` environment variables to instruct the container to enable SSL in Apache and MySQL, then it mounts the certificate files.

```
docker container run <some-options> \
  -e APACHE_SSL_ON=1 \
  -v "${HOME}/certs/apache/apiportal.crt":/opt/axway/apiportal/certs/apache/apache.crt:ro \
  -v "${HOME}/certs/apache/apiportal.key":/opt/axway/apiportal/certs/apache/apache.key:ro \
  -e MYSQL_SSL_ON=1 \
  -v "${HOME}/certs/mysql/ca.pem":/opt/axway/apiportal/certs/mysql/mysql-ca.pem:ro \
  -v "${HOME}/certs/mysql/client-cert.pem":/opt/axway/apiportal/certs/mysql/mysql-client-cert.pem:ro \
  -v "${HOME}/certs/mysql/client-key.pem":/opt/axway/apiportal/certs/mysql/mysql-client-key.pem:ro \
  axway/apiportal:X.X.X
```

You can mix certificates in mounted files and environment variables, but note that values from mounted certificate files override the ones from environment variables.

```
docker container run <some-options> \
  -e APACHE_SSL_ON=1 \
  -e APACHE_SSL_CERT="base64:$(cat ~/certs/apiportal.crt)" \
  -v "${HOME}/certs/apache/apiportal.key":/opt/axway/apiportal/certs/apache/apache.key:ro \
  axway/apiportal:X.X.X
```

You can simplify the process by creating the following file structure under a directory of your choice, for example `${HOME}/certs`:

```
${HOME}/certs/
├── apache/
│   ├── apache.crt
│   └── apache.key
└── mysql/
    ├── mysql-ca.pem
    ├── mysql-client-cert.pem
    └── mysql-client-key.pem
```

Ensure that you named the files exactly as they are expected inside the container. Now, you can configure the container using certificates by mounting the whole `"${HOME}/certs` directory:

```
docker container run <some-options> \
  -e APACHE_SSL_ON=1 -e MYSQL_SSL_ON=1 \
  -v "${HOME}/certs":/opt/axway/apiportal/certs:ro \
  axway/apiportal:X.X.X
```

## Create data volumes to persist data

By default, API Portal container does not create volumes for data persistence. But, you might want to create Docker data volumes to persist API Portal customization data, prevent data loss when the container reboots or crashes, or when you are upgrading or setting up HA for an API Portal Docker deployment. If you are running API Portal in containers for a demo or test, there is no need to create data volumes.

The data volumes are stored in the Docker host machine, and as such they consume disk space. So, we recommend you to delete unused data volumes regularly.

The following list describes which API Portal assets you should store in a Docker volume to preserve the customizations during upgrade or HA setup of an API Portal Docker deployment:

* `/etc/apiportal` - API Portal `etc` directory. Used for encryption key secret for API Manager and Elasticsearch passwords.
* `/opt/axway/apiportal/enckey` - Encryption key directory. Used by Public API mode.
* `/opt/axway/apiportal/tasks` - Regular tasks directory. Used by Elasticsearch scheduler.
* `/opt/axway/apiportal/htdoc/images` - Images uploaded by API Portal users or Admins.
* `/opt/axway/apiportal/htdoc/language` - API Portal translations.
* `/opt/axway/apiportal/htdoc/templates` - Joomla! templates.
* `/opt/axway/apiportal/htdoc/administrator/language` - Joomla! admin panel translations.
* `/opt/axway/apiportal/htdoc/administrator/components/com_apiportal/assets/cert` - Certificates for API Manager.

Do not modify the content of the following folders, as they will be overwritten during upgrade:

* `/opt/axway/apiportal/htdoc/templates/purity_iii`
* `/opt/axway/apiportal/htdoc/language/en-GB`
* `/opt/axway/apiportal/htdoc/language/overrides`
* `/opt/axway/apiportal/htdoc/administrator/language`

The following is an example of how you can create data volumes:

```
# create volumes
docker volume create apiportal-etc
docker volume create apiportal-enckey
docker volume create apiportal-tasks
docker volume create apiportal-images
docker volume create apiportal-language
docker volume create apiportal-templates
docker volume create apiportal-adm-language
docker volume create apiportal-certs

# start API Portal container using the created volumes
docker container run \
  -v apiportal-etc:/etc/apiportal \
  -v apiportal-enckey:/opt/axway/apiportal/enckey \
  -v apiportal-tasks:/opt/axway/apiportal/tasks \
  -v apiportal-images:/opt/axway/apiportal/htdoc/images \
  -v apiportal-language:/opt/axway/apiportal/htdoc/language \
  -v apiportal-templates:/opt/axway/apiportal/htdoc/templates \
  -v apiportal-adm-language:/opt/axway/apiportal/htdoc/administrator/language \
  -v apiportal-certs:/opt/axway/apiportal/htdoc/administrator/components/com_apiportal/assets/cert \
  <more-options>
```

As API Portal container runs as a non-root user. You must ensure that mounted directories are readable and writable by user with id `1048`. This user is not required to exist in the host machine.

## RHEL API Portal software installation versus API Portal running in a docker container

* [Elasticsearch scheduling](/docs/apim_installation/apiportal_install/install_software_elastic/#configure-a-schedule-to-push-data-to-elasticsearch) - API Portal software installation accepts non-standard cron expression syntax if the expression is supported by the cron version installed on the server, whereas in the Docker image only standard syntax is supported. For example, values like `@daily` or `@reboot` are not allowed.
* [Public API Mode](/docs/apim_administration/apiportal_admin/public_api_configure) - In API Portal software installations, an encryption key directory must be generated with `apiportal_encryption.sh` script or with an option in API Portal installer to enable the Public API Mode feature, whereas the Docker image include pre-generated encryption directory (For more information, see [Create data volumes to persist data](#create-data-volumes-to-persist-data) section).