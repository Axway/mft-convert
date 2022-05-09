{
"title": "Unattended installation",
  "linkTitle": "Unattended installation",
  "weight": "100",
  "date": "2019-08-09",
  "description": "Install or uninstall API Portal in unattended mode."
}

This page describes how you can install and uninstall API Portal in unattended mode, also known as silent more. The following sections cover how you can perform the installation either by entering all required options for the installation and unninstallation scripts in the command line, or using a configuration file.

## Install API Portal by entering all the script options

To install API Portal in unattended mode by entering all the script options in the command line:

1. Download the installation package for your OS from [Axway Support](https://support.axway.com/) and upload it to your host machine.
2. Log in to the host machine as the `root` user.
3. Extract the installation package:

   ```shell
   tar xpvzf <package_name>.tgz
   ```
4. Run the installation script with the appropriate options. For example:

   ```shell
   ./apiportal_install.sh --mysql-ssl=n --install-path="/opt/axway/apiportal/htdoc" --mysql-database=joomla --mysql-host=localhost --mysql-port=3306 --mysql-username=apiportal --mysql-password=password --mysql-encrypt-password=y --mysql-password-passphrase=passphrase --weak-mysql-password=y --initial-ha-instance=n --php-ini="/etc/" --apache-config="/etc/httpd/conf.d/" --use-encryption-key=n --use-ssl=y --ssl-type=2 --restart-apache=y
   ```

For more information about the options used with the script, see [Scripts reference](#scripts-reference).

## Uninstall API Portal by entering all the script options

To uninstall API Portal in unattended mode by entering all the script options in the command line:

1. Log in to the host machine as the `root` user.
2. Change to the directory containing the API Portal installation package.
3. Run the uninstall script with the appropriate script options. For example:

   ```shell
   ./apiportal_uninstall.sh --mysql-keep=n --mysql-database=testDB --mysql-host=localhost --mysql-port=3306 --mysql-username=root --mysql-password=Admin@123
   ```

For more information about the options used with the script, see [Scripts reference](#scripts-reference).

Watch this video to learn how to install and uninstall API Portal using unattended mode:

{{< youtube 1yijU63_z6g >}}

## Install and uninstall API Portal using a configuration file

You can use a configuration file to install or uninstall API Portal in unattended mode. Using a file provides the following advantages:

* Allows you to create a reproducible installation configuration, which you can easily update every time that you need to reinstall or uninstall your API Portal.
* Decreases the chance of making typos, since you do not type the options directly in the command line, and running into errors.

The following is an example that you can copy and paste, then edit the values, and use the file with the `install` or `uninstall` commands:

```
# Accepts yY/nN. Flag indicating whether user wants to
# continue when PHP could not be detected in Apache.
apache-without-php=

# Accepts yY/nN. Flag indicating whether API Portal database
# will be removed. This option only takes effect on
# API Portal uninstallation. All other mysql options are
# disregarded when the flag is enabled.
mysql-keep=

# Accepts yY/nN. Flag indicating whether to use MySQL
# in SSL mode.
mysql-ssl=

# Accepts 1 or 2. Indicates the method used when SSL mode
# for MySQL is wanted.
# * 1 - One-way authentication
# * 2 - Two-way authentication
mysql-ssl-method=

# The install path for API Portal.
# Example: /opt/axway/apiportal/htdoc
install-path=

# The database that will be used by API Portal.
mysql-database=

# Database host.
mysql-host=

# Database port. Example: 3306
mysql-port=

# Database user.
mysql-username=

# Database password.
mysql-password=

# Accepts yY/nN. Flag indicating whether DB password
# is to be stored encrypted or not.
mysql-encrypt-password=

# Passphrase to be used to encrypt and decrypt database
# password.
mysql-password-passphrase=

# Accepts yY/nN. Flag indicating whether installation
# should continue when MySQL password is weak.
weak-mysql-password=

# Accepts yY/nN. Flag indicating whether this is the
# initial instance of HA setup.
initial-ha-instance=

# Accepts yY/nN. Flag indicating whether the HA setup is
# wanted. Use only for instances that are not initial.
# For the initial instance use the --initial-ha-instance
# option.
ha-instance=

# The directory where php.ini file is located.
# Example: /etc
php-ini=

# The directory where custom apache configuration files are
# located.
# Example: /etc/httpd/conf.d
apache-config=

# Accepts yY/nN. Flag indicating whether encryption key is
# wanted. This option is required when public API mode is
# going to be used.
use-encryption-key=

# The place where the encryption key will be stored.
# Example: /home/encryption/key.
# The last segment is the filename where the key will
# be stored. In this example it will be called 'key'.
# Used when yY is selected for --use-encryption-key option.
encryption-key=

# Accepts yY/nN. Flag indicating whether API Portal will be
# served by SSL.
use-ssl=

# Accepts 1 or 2. Indicates what SSL type is wanted.
# * 1 - Custom certificate and private key will be provided.
# * 2 - Use self-signed certificate.
ssl-type=

# Path to the SSL certificate. Used when option 1 SSL type
# is selected.
ssl-certificate=

# Path to the private key. Used when option 1 SSL type is
# selected.
private-key=

# The hostname of API Portal. Used when option 1 SSL type
# is selected.
hostname=

# Accepts yY/nN. Flag indicating whether Apache restart is
# wanted after installation (when the apache service could
# be correctly detected; otherwise manual restart of Apache
# is required).
restart-apache=
```

The same file can be generated by the `apiportal_install.sh` script using the following command:

```shell
./apiportal_install.sh --gen-optionfile > options.conf
```

The following rules apply to the configuration file:

* Each line in the file must be in `key=value` format.
* Lines beginning with # are processed as comments, they are ignored.
* Blank lines are ignored.
* There is no special handling of quotation marks, they are part of the `value`.

After the configuration file is complete, you can use it with API Portal `install` and `uninstall` scripts.

Example:

```shell
# install API Portal
./apiportal_install.sh --optionfile=options.conf
```

```shell
# uninstall API Portal
./apiportal_uninstall.sh --optionfile=options.conf
```

You can use inline options to override the configuration file values.

```shell
./apiportal_install.sh --optionfile=options.conf --mysql-password="<db-password>"
```

This can be useful to avoid adding sensitive data to the configuration file, or when you want to use same file for multiple environments, like different database servers.

{{< alert title="Note" color="primary" >}}Do not add sensitive information to the configuration file.{{< /alert >}}

Watch this video to learn how to install and uninstall API Portal using a configuration file:

{{< youtube HqQ77Cj2s5E >}}

## Encrypt the Public API user password in unattended mode

To encrypt the Public API mode user password in unattended mode:

1. Log in to the host machine as the `root` user.
2. Change to the directory containing the API Portal installation package.
3. Run the encryption script with the appropriate script options. For example:

   ```
   ./apiportal_encryption.sh --encryption-key="/publicapi/encryption/user.key"
   ```

   Install options:

   * `--encryption-key`: The place where the encryption key is stored. Example: `/home/encryption/key`. The last segment is the file name (in this example it is called `key`).
   * `-h` or `--help`: Prints all available options for the script.

## Scripts reference

The following lists the options that you can use with the install and uninstall scripts:

* `--apache-without-php`: Accepts yY/nN. Indicates whether to continue when PHP could not be detected in Apache.
* `--mysql-keep`: Accepts yY/nN. Flag indicating whether API Portal database will be removed. This option only takes effect on API Portal uninstallation. All other mysql options are disregarded when the flag is enabled.

* `--mysql-ssl`: Accepts yY/nN. Indicates whether to use MySQL in SSL mode.
* `--mysql-ssl-method`: Indicates the method used when SSL mode for MySQL is wanted. Accepts `1` (one-way authentication) or `2` (two-way authentication).
* `--install-path`: The install path for API Portal. Example: `/opt/axway/apiportal/htdoc`.
* `--mysql-database`: The database to use for API Portal.  
* `--mysql-host`: Database host.
* `--mysql-port`: Database port. Example: `3306`.
* `--mysql-username`: Database user.
* `--mysql-password`: Database password.
* `--mysql-encrypt-password`: Accepts yY/nN. Indicates whether to stored the DB password encrypted.
* `--mysql-password-passphrase`: Passphrase to be used to encrypt and decrypt database password. Used when `--mysql-encrypt-password` is set to yY.
* `--weak-mysql-password`: Accepts yY/nN. Indicates whether installation should continue when MySQL password is weak.
* `--ha-instance`        : Accepts yY/nN. Indicates whether to use HA setup. Use only for instances that are not initial. For the initial instance use the `--initial-ha-instance` option.
* `--initial-ha-instance`: Accepts yY/nN. Indicates whether this is the initial instance for HA setup.
* `--php-ini`            : The directory where `php.ini` file is located. Example: `/etc`.
* `--apache-config`      : The directory where the Apache configuration files are located. Example: `/etc/httpd/conf.d`.
* `--use-encryption-key` : Accepts yY/nN. Indicates whether to use an encryption key. This option is required for public API mode.
* `--encryption-key`     : Directory where the encryption key is stored. Example: `/home/encryption/key`. The last segment is the file name (in this example it is called `key`). sed when `--use-encryption-key` is set to yY.
* `--use-ssl`            : Accepts yY/nN. Indicates whether API Portal will be served by SSL.
* `--ssl-type`           : Indicates what SSL type to use. Accepts `1` (use custom certificate and private key) or `2` (use self-signed certificate).
* `--ssl-certificate`    : Path to the SSL certificate. Used when SSL type `1` is selected.
* `--private-key`        : Path to the private key. Used when SSL type `1` is selected.
* `--private-key-passphrase`: The passphrase of the private key. Used when SSL type `1` is selected and the private key has passphrase.
* `--passphrase-path`: Directory where the passphrase will be stored. The last segment will be the filename where the passphrase will be stored. The file is required to setup pache to start silently (without asking for passphrase). Used when SSL type `1` is selected and the private key has passphrase.
* `--hostname`: The hostname of API Portal. Used when SSL type `1` is selected.
* `--restart-apache`: Accepts yY/nN. Indicates whether Apache restart is wanted after installation (when the apache service could be correctly detected; otherwise manual restart f Apache is required).
* `--optionfile`         : Reads installation options from a file. Example: `--optionfile=basic.conf`. Inline options take precedence over the options from the configuration file.
* `--gen-optionfile`     : Prints out the file to stdout. By redirecting the output to a file instead, you can quickly generate a dummy configuration file. This option is not equired by the installation process.

To see the complete list of options, run one of the following command:

```
./apiportal_install.sh --help
```

```
./apiportal_uninstall.sh --help
```
