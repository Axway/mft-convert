{
"title": "Software installation prerequisites",
  "linkTitle": "Prerequisites",
  "weight": "10",
  "date": "2019-08-09",
  "description": "Your system must meet these prerequisites before you can install API Portal."
}
## Internet connection

Installing API Portal dependencies requires an Internet connection. Offline installation is not currently supported.

## Hardware requirements

The minimum hardware requirements are:

* 2 Ghz Dual Core Intel Core or AMD Opteron or faster
* 8 GB RAM
* 40 GB free disk space

## Software requirements

API Portal has the following requirements for software components:

### Operating system

You must have Red Hat Enterprise Linux (RHEL) 7/8 or CentOS 7/8 installed.

### Database

You must have one of the following databases installed:

* MySQL 5.7 or MySQL 8

  MySQL 8 is only proved to be working in combination with PHP 8.0. API Portal has not yet been tested with MySQL 8 and PHP lower than version 8.0.

* MariaDB 10.4 or later

### PHP

API Portal requires PHP 7.3 through 8.0. We strongly recommend you to use only [officially supported PHP versions](https://www.php.net/supported-versions.php).

In addition, you must have the following PHP packages installed:

* `php-cli`
* `php-common`
* `php-gd`
* `php-intl`
* `php-json`
* `php-mbstring`
* `php-mysqld` or `php-mysqli`
* `php-openssl`
* `php-pdo`
* `php-xml`
* `php-zip`

{{< alert title="Note" color="primary" >}}Names of the packages may differ depending on the operating system.{{< /alert >}}

### Other software

API Portal requires the following to be installed:

* API Manager 7.7 (API Portal and API Manager releases must match.)

  The monitoring feature of API Portal, which enables your API consumers to monitor application and API usage, requires a connected API Manager with monitoring metrics enabled.
* Apache 2.4
* OpenSSL

### Install dependencies

For more information on installing dependencies, see:

* [Install Apache HTTP Server and PHP](/docs/apim_installation/apiportal_install/install_dependencies)
* [Install and configure database server](/docs/apim_installation/apiportal_install/install_software_configure_database)
* [Configure Redis cache](/docs/apim_installation/apiportal_install/install_software_redis)
