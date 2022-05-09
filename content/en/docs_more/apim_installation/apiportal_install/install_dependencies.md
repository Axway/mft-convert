{
"title": "Install Apache HTTP server and PHP",
  "linkTitle": "Install Apache HTTP server and PHP",
  "weight": "20",
  "date": "2019-08-09",
  "description": "The API Portal installation script does not install specific dependencies (such as Apache HTTP server and PHP) that are required by API Portal, so you must install these dependencies yourself before you install API Portal."
}
## Install dependencies from RedHat Software Collections on RHEL 7

On a Red Hat Enterprise Linux (RHEL7) installation, the default PHP version available in the official repository is 5.4. However, API Portal only supports PHP 7.1+. You can use RedHat Software Collections (RHSCL) to install PHP 7.1+.

### Enable RedHat Software Collections

Before installing Apache HTTP server and PHP you must first enable RHSCL:

```bash
sudo subscription-manager repos --enable rhel-server-rhscl-7-rpms
sudo yum clean all
```

### Install Apache HTTP server on RHEL7

1. Install Apache from RHSCL:

   ```bash
   sudo yum install httpd24-httpd httpd24-httpd-tools httpd24-mod_ssl
   ```
2. Verify the `/usr/bin/httpd` directory does not exist. If it does, rename it and run:

   ```bash
   sudo ln -s $(which httpd) /usr/bin/httpd
   ```
3. Make Apache available in any bash session by default:

   ```bash
   echo "source scl_source enable httpd24" | sudo tee -a /etc/profile.d/scl-httpd24.sh
   source /etc/profile.d/scl-httpd24.sh
   sudo ln -s $(which httpd) /usr/bin/httpd
   ```
4. Verify Apache is now available:

   ```bash
   httpd -v
   ```
5. Enable and start Apache service:

   ```bash
   sudo systemctl enable --now httpd24-httpd
   ```
6. Verify that Apache service is active and running:

   ```bash
   systemctl status httpd24-httpd
   ```

### Install PHP on RHEL7

You must install the latest PHP version provided by the RHSCL.

1. Install all required PHP packages:

   ```bash
   sudo yum install rh-php73 rh-php73-php rh-php73-php-cli rh-php73-php-common rh-php73-php-gd rh-php73-php-json rh-php73-php-intl rh-php73-php-mbstring rh-php73-php-mysqlnd rh-php73-php-pdo rh-php73-php-xml rh-php73-php-zip
   ```
2. Verify the `/usr/bin/httpd` directory does not exist. If it does, rename it and run:

   ```bash
   sudo ln -s $(which httpd) /usr/bin/httpd
   ```
3. Enable PHP in bash:

   ```bash
   echo "source scl_source enable rh-php73" | sudo tee -a /etc/profile.d/scl-rh-php73.sh
   source /etc/profile.d/scl-rh-php73.sh
   sudo ln -s $(which php) /usr/bin/php
   ```
   If `/usr/bin/php` already exists, please rename it and execute `sudo ln -s $(which php) /usr/bin/php` again.

4. Verify that PHP is available:

   ```bash
   php -v
   ```
5. Verify that `php7_module` is enabled in Apache:

   ```bash
   httpd -M | grep php7
   ```
6. Restart Apache and verify it is running:

   ```bash
   sudo systemctl restart httpd24-httpd
   systemctl status httpd24-httpd
   ```

## Install dependencies from community repository EPEL with Remi for RHEL 8 and CentOS 7/8

RHEL 8 and CentOS do not offer the latest PHP version in the default repositories. To install the latest PHP, we recommend using the Extra Packages for Enterprise Linux (EPEL) repository.

### Enable EPEL with Remi

To install and enable EPEL with Remi:

```bash
# for CentOS 7
sudo yum install epel-release yum-utils
sudo yum install http://rpms.remirepo.net/enterprise/remi-release-$(rpm -E '%{rhel}').rpm
```

```bash
# for CentOS 8
sudo dnf install epel-release yum-utils
sudo dnf install http://rpms.remirepo.net/enterprise/remi-release-$(rpm -E '%{rhel}').rpm
sudo dnf update
```

```bash
# for RHEL 8
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-$(rpm -E '%{rhel}').noarch.rpm

sudo subscription-manager repos --enable codeready-builder-for-rhel-8-$(arch)-rpms

sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-$(rpm -E '%{rhel}').rpm

sudo dnf install dnf-utils
```

### Install Apache HTTP server on CentOS/RHEL 8

1. Install Apache HTTP Server and its SSL module:

   ```bash
   # for CentOS 7
   sudo yum install httpd mod_ssl
   ```

   ```bash
   # for CentOS 8 / RHEL 8
   sudo dnf install httpd mod_ssl
   ```
2. Enable and start the Apache service:

   ```bash
   # for CentOS 7/8 and RHEL 8
   sudo systemctl enable --now httpd
   ```

    If you get the error message: `SSLCertificateFile: file '/etc/pki/tls/certs/localhost.crt' does not exist or is empty` you can resolve it by generating a self-signed certificate:

   ```
   openssl req -newkey rsa:4096 -x509 -sha256 -days 3600 -nodes \
   -out "/etc/pki/tls/certs/localhost.crt" \
   -keyout "/etc/pki/tls/private/localhost.key"
   ```

    Then, run `sudo systemctl enable --now httpd` again.
3. Verify that Apache service is active and running:

   ```bash
   # for CentOS 7/8 and RHEL 8
   systemctl status httpd
   ```
4. Open the TCP port 80 and 443 in the local firewall

   ```bash
   # for CentOS 7/8 and RHEL 8
   sudo firewall-cmd --permanent --add-port=80/tcp
   sudo firewall-cmd --permanent --add-port=443/tcp
   sudo firewall-cmd --reload
   ```

### Install PHP on CentOS and RHEL 8

1. Install PHP:

   ```bash
   # for CentOS 7
   sudo yum-config-manager --enable remi-php74
   sudo yum install php php-cli php-common php-gd php-json php-intl php-mbstring php-mysqlnd php-pdo php-xml php-pecl-zip
   ```

   ```bash
   # for CentOS 8
   sudo dnf module reset php
   sudo dnf module install php:remi-7.4

   sudo dnf install php php-cli php-common php-gd php-json php-intl php-mbstring php-mysqlnd php-pdo php-xml php-pecl-zip
   ```

   ```bash
   # for RHEL 8
   sudo dnf module reset php
   sudo dnf module install php:remi-7.4

   sudo dnf install php php-cli php-common php-gd php-json php-intl php-mbstring php-mysqli php-openssl php-pdo php-xml php-zip
   ```
2. Verify that PHP was installed. If the command fails, restart the bash session:

   ```bash
   # for CentOS 7/8 and RHEL 8
   php -v
   ```
3. Locate the `mpm` configuration file and make sure that line `LoadModule mpm_prefork_module modules/mod_mpm_prefork.so` is uncommented and all the other lines are commented out.

   ```bash
   # for CentOS 7/8 and RHEL 8
   find /etc/httpd -name '*-mpm.conf'
   ```
4. Verify that `php7_module` of Apache is enabled:

   ```bash
   # for CentOS 7/8 and RHEL 8
   httpd -M | grep php7
   ```
5. Restart Apache and verify that it is working:

   ```bash
   # for CentOS 7/8 and RHEL 8
   sudo systemctl restart httpd
   systemctl status httpd
   ```