{
"title": "Install Apache HTTP server and PHP",
  "linkTitle": "Install Apache HTTP server and PHP",
  "weight": "20",
  "date": "2019-08-09"
}

The API Portal installation script does not install specific dependencies (such as Apache HTTP server and PHP) that are required by API Portal, so you must install these dependencies yourself before you install API Portal

## Install dependencies from EPEL and Remi

RHEL and CentOS do not offer the latest PHP version in their default repositories. To install the latest PHP, we recommend using the Extra Packages for Enterprise Linux (EPEL) repository as [RedHat Software Collections](https://www.softwarecollections.org/en/) (RHSCL) currently does not support PHP 8.0.

Follow the next sections to install API Portal dependencies from community repository EPEL with Remi for RHEL 7/8 and CentOS 7/8. The overall steps are:

1. Enable EPEL with Remi.
2. Install Apache HTTP server.
3. Install PHP.

### Enable EPEL with Remi

Install and enable EPEL with Remi:

```bash
# for CentOS 7
sudo yum install epel-release yum-utils
sudo yum install http://rpms.remirepo.net/enterprise/remi-release-$(rpm -E '%{rhel}').rpm
```

```bash
# for RHEL 7
yum install yum-utils
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-$(rpm -E '%{rhel}').noarch.rpm
yum install http://rpms.remirepo.net/enterprise/remi-release-$(rpm -E '%{rhel}').rpm
```

```bash
# for CentOS 8
sudo dnf install epel-release
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

### Install Apache HTTP server

Follow these steps to install Apache HTTP server:

1. Install Apache HTTP Server and its SSL module:

   ```bash
   # for CentOS 7 / RHEL 7
   sudo yum install httpd mod_ssl
   ```

   ```bash
   # for CentOS 8 / RHEL 8
   sudo dnf install httpd mod_ssl
   ```
2. Enable and start the Apache service:

   ```bash
   # for CentOS 7/8 and RHEL 7/8
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
   # for CentOS 7/8 and RHEL 7/8
   systemctl status httpd
   ```
4. Open the TCP port 80 and 443 in the local firewall

   ```bash
   # for CentOS 7/8 and RHEL 7/8
   sudo firewall-cmd --permanent --add-port=80/tcp
   sudo firewall-cmd --permanent --add-port=443/tcp
   sudo firewall-cmd --reload
   ```

### Install PHP

Follow these steps to install PHP:

1. Install PHP:

   ```bash
   # for CentOS 7 / RHEL 7
   sudo yum-config-manager --enable remi-php80
   sudo yum install php php-cli php-common php-gd php-json php-intl php-mbstring php-mysqlnd php-pdo php-xml php-pecl-zip
   ```

   ```bash
   # for CentOS 8
   sudo dnf module reset php
   sudo dnf module install php:remi-8.0
   sudo dnf install php php-cli php-common php-gd php-json php-intl php-mbstring php-mysqlnd php-pdo php-xml php-pecl-zip
   ```

   ```bash
   # for RHEL 8
   sudo dnf module reset php
   sudo dnf module install php:remi-8.0
   sudo dnf install php php-cli php-common php-gd php-json php-intl php-mbstring php-mysqli php-openssl php-pdo php-xml php-zip
   ```
2. Verify that PHP was installed. If the command fails, restart the bash session:

   ```bash
   # for CentOS 7/8 and RHEL 7/8
   php -v
   ```
3. Locate the `mpm` configuration file and make sure that line `LoadModule mpm_prefork_module modules/mod_mpm_prefork.so` is uncommented and all the other lines are commented out.

   ```bash
   # for CentOS 7/8 and RHEL 7/8
   find /etc/httpd -name '*-mpm.conf'
   ```
4. Verify that `php7_module` of Apache is enabled:

   ```bash
   # for CentOS 7/8 and RHEL 7/8
   httpd -M | grep php
   ```
5. Restart Apache and verify that it is working:

   ```bash
   # for CentOS 7/8 and RHEL 7/8
   sudo systemctl restart httpd
   systemctl status httpd
   ```
