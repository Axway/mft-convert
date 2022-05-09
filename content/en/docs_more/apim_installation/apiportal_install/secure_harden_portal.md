{
"title": "Secure API Portal",
  "linkTitle": "Secure API Portal",
  "weight": "80",
  "date": "2019-08-09",
  "description": "Secure and harden your API Portal environment after installation."
}
Perform the following steps after installation to ensure that your API Portal environment is secure from internal and external threats:

1. Apply the latest service pack (SP) available for this version, as it might contain important security updates. For details, see [Update API Portal](/docs/apim_installation/apiportal_install/install_service_pack/).
2. Search [Axway Support](https://support.axway.com/) for any KB articles relating to this version, as these might contain valuable security recommendations.
3. Change the default Joomla! administrator credentials for logging in to the Joomla! Administrator Interface (JAI) (`https://<API Portal host>/administrator`). Use a different user name and a strong password.
4. Complete all of the procedures detailed in the following sections.

## Configure the SSL certificate

To enable SSL on API Portal, you must configure Apache to use the correct certificate:

1. Open the `/etc/httpd/conf.d/apiportal.conf` file.
2. Change `SSLCertificateFile` and `SSLCertificateKeyFile` to point to your CA certificate and key files.
3. Restart Apache.

For more details on API Portal certificate management, see the [API Management Security Guide](/docs/apimgmt_security/).

## Disable TLS 1.0 and TLS 1.1 on Apache

On an API Portal software installation, the Apache web server has TLS versions 1.0 and 1.1 enabled in addition to the TSL 1.2 that API Portal uses. Because TLS 1.0 and 1.1 have security vulnerabilities, it is recommended to disable them.

1. To check which TLS versions are enabled, scan your API Portal port. By default, API Portal uses port `443` for secure connections:

   ```
   sslscan <API Portal IP address>:<your https port>
   ```
2. To disable TLS 1.0. and 1.1, open the following file: `/etc/httpd/conf.d/apiportal.conf`
3. Add the following SSL protocol definition for the secure connection:

   ```
   <VirtualHost *:443>
      SSLEngine on
      SSLCertificateFile "/etc/httpd/conf/server.crt"
      SSLCertificateKeyFile "/etc/httpd/conf/server.key"
      SSLProtocol TLSv1.2
      Header always append X-Frame-Options SAMEORIGIN
       ...
   </VirtualHost>
   ```
4. Restart Apache.
5. Run the `sslscan` again on your API Portal port to check that TLS 1.0 and 1.1 have been disabled.

## Protect Joomla! from direct Internet access

To counter a session fixation vulnerability in Joomla!, it is recommended that you protect the Joomla! Administrator Interface (JAI) from direct Internet access.

1. Open the `/etc/httpd/conf.d/security.conf` file.
2. Add an access restriction directive for the `/administrator` location. Specify the internal IP address range that is allowed to access JAI. For example:

   ```
   ServerTokens ProductOnly
   ServerSignature Off
       <Location /administrator>
           Order deny,allow
           deny from all
           allow from 10.232.14.
       </Location>
   ```
3. To restart the web server configuration, enter the following:

   ```
   # /etc/init.d/apache2 reload
   ```

## Encrypt database password

If you did not choose to encrypt your database password during the installation process, you can use the `apiportal_db_pass_encryption.sh` script to encrypt the password at any time. The script is available both from API Portal installation and upgrade packages.

To encrypt your database password, run:

```
# sh apiportal_db_pass_encryption.sh
```

You will be prompted to enter a passphrase and your database password.

The script uses the passphrase to encrypt and decrypt the database password on each connection request. The database password is stored encrypted in the `<API_Portal_install_path>/configuration.php` file. Only the password is decrypted on each connection request, not the whole payload, so no significant performance impact is expected.

Note that if you encrypt your database password, you cannot use the [database secure connection](/docs/apim_installation/apiportal_install/install_software_configure_database/#configure-the-database-server-for-secure-connection) option.

## Update the database password

This section shows how you can update an encrypted database password.

### Update an encrypted password

To update an encrypted password:

1. Change the database password in your database server.
2. Run the `apiportal_db_pass_encryption.sh` script and provide the new database password to use.

### Update an encrypted password to plaintext password

To update an encrypted password to plaintext password:

1. Change the database password in your database server.
2. Edit your `configuration.php` file.
3. Locate the line that starts with `public $password =` and replace its value with the plaintext MySQL password. Ensure to follow [PHP quoting rules](https://www.php.net/manual/en/language.types.string.php) for any special characters in the password.
4. Locate the `public $dbtype = 'mysqli_encrypted'` line and replace it with `public $dbtype = 'mysqli'`.

To encrypt your plaintext password, see [Encrypt database password](#encrypt-database-password).

## Limit the number of failed login attempts

To protect API Portal and Joomla! from brute force attacks, you can limit the number of failed login attempts that API Portal or Joomla! allows:

1. In the JAI, click **Components > API Portal > Login Protection**.
2. Click **Yes** to enable login protection for API Portal.
3. Enter a value for the number of failed login attempts before a ReCaptcha is displayed.
4. Enter a value for the number of failed login attempts before the user account is locked.
5. Enter a value, in seconds, for how long the user account is locked.
6. Click **Yes** to enable locking by IP address. When this setting is enabled login attempts are blocked from the same IP address for the lock time specified even if correct user credentials are entered.
7. Click **Save**.

You can enable **user account** locking and **IP address** locking independently or in combination. For example, if you enable user account locking and IP address locking for 5 minutes after 2 failed login attempts, `UserA` will be locked for 5 minutes after entering 2 incorrect passwords, and any other user (for example, `UserB`) will also be unable to log in for 5 minutes from the same IP address, even if they provide correct user credentials.

## Add trusted OAuth hosts

To restrict API Portal users from accessing unauthorized OAuth endpoints, you can enter a list of permitted OAuth hosts in the OAuth whitelist:

1. In the JAI, click **Components > API Portal > Whitelisting**.
2. In the **OAuth Whitelist** field, enter the host names or IP addresses of the trusted OAuth hosts (separated by new lines). Do not enter API Manager hosts as these are added to the whitelist automatically.
3. Click **Save**.

If you do not add your trusted OAuth hosts to this field, all requests to those hosts will be rejected by API Portal.

## Add trusted API hosts

If you have APIs that are virtualized and published on a host other than an API Manager host, you can enter a list of permitted API hosts in the API whitelist:

1. In the JAI, click **Components > API Portal > Whitelisting**.
2. In the **API Whitelist** field, enter the host names or IP addresses of the trusted API hosts (separated by new lines). Do not enter API Manager hosts as these are added to the whitelist automatically.
3. Click **Save**.

If you do not add your trusted API hosts to this field, all requests to those hosts will be rejected by API Portal.

## Configure Apache

### Update apiportal.conf

Add security headers to the `apiportal.conf` file (located in `/etc/httpd/conf.d/`).

In the virtual host directive add the following:

```
Header edit Set-Cookie "(?i)^((?:(?!;\s?HttpOnly).)+)$" "$1; HttpOnly"
Header edit Set-Cookie "(?i)^((?:(?!;\s?Secure).)+)$" "$1; Secure"
Header unset X-Frame-Options
Header always append X-Frame-Options SAMEORIGIN
Header set X-XSS-Protection "1; mode=block"
Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains;"
Header set X-Content-Type-Options nosniff
Header set Referrer-Policy "same-origin"
```

{{< alert title="Note" color="" >}}`SameSite` attribute is not compatible with SSO.

If you are not using SSO, we recommend you to add `SameSite` to the host directive so that the cookies are sent only in First-Party (API Portal) context, and not along with requests initiated by Third-Party websites.

```
`Header edit Set-Cookie "(?i)^((?:(?!;\s?SameSite=Strict).)+)$" "$1; SameSite=Strict"`
```

{{< /alert >}}

You should only use the HSTS header if you have configured SSL.

### Update security.conf

Ensure that the `security.conf` file (located at `/etc/httpd/conf.d/`) exists and that it contains the following directives:

```
ServerTokens ProductOnly
ServerSignature Off
HostnameLookups Off
TraceEnable Off
UseCanonicalName Off
```

### Set ServerName to proper FQDN

To protect your web server from a vulnerability giving remote attackers the ability to attain your internal IP address or internal network name, set `ServerName` to a proper FQDN.

### Restart Apache

Restart Apache after modifying the `apiportal.conf` and `security.conf` files.

## Configure PHP

Find the location of your `php.ini` file. For example, run the command:

```
php -i | grep php.ini
```

In the resulting list of files, the `php.ini` listed as the `Loaded Configuration File` is the correct file to edit.

Update the file with the following options:

```
- expose_php = 0
- display_errors = Off
- disable_functions = "passthru,shell_exec,system"
- allow_url_include = Off
- session.cookie_httponly = 1
- session.cookie_secure = On
- session.cookie_samesite = "Strict"
- open_basedir = "/opt/axway/apiportal/htdoc:/tmp"
```

You should only set `session.cookie_secure` to `On` if you have configured SSL.

The `open_basedir` option must be added after the installation is finished. Set `open_basedir` to a list of directories (use `:` to separate directories):

* API Portal root directory
* Value of `upload_tmp_dir` or `/tmp` if it is empty
* New location of log files if you changed them according to [Change the location of API Portal log files](#change-the-location-of-api-portal-log-files)

After updating `php.ini`, restart Apache.

## Configure MySQL

MySQL comes with a hardening script to check database server security and remove some default settings. To run the script:

```
mysql_secure_installation
```

If you do not need to access your database from another machine, bind the MySQL service on localhost only (of the host from which you are going to access it). For example, edit the configuration file `my.cnf` and set:

```
bind-address = 127.0.0.1
```

API Portal users should only have access to the databases that they need to run.

## Configure Joomla! Administrator Interface (JAI)

1. Log in to JAI.
2. Select **System > Global Configuration**.
3. Click the **Site** tab.
4. In **Cookie Settings** enter your API Portal domain name (for example, `myapiportal.com`) for **Cookie Domain** and enter `/` for **Cookie Path**.
5. In **Metadata Settings** set **Show Joomla Version** to `No`.
6. Click the **System** tab and in **Debug Settings** set **Debug System** to `No`.
7. Click the **Server** tab and in **Server Settings** set **Error Reporting** to `None` and **Force HTTPS** to `Entire site` (if you have configured SSL).
8. Select **Users** from the left navigation bar.
9. Click the **User Options** tab and set **Allow User Registration** to `No`.

## Enable two-factor authentication

Joomla! two-factor authentication adds an extra layer of security to your portal by asking the user for a single-use secret code.

After you install API Portal with Joomla! version 3.2 or higher, Joomla! backend shows a notice for you to check the post-installation messages. From the post-Installation message screen, click **Enable Two-Factor Authentication** to enable two-factor authentication on your API Portal.

To configure the two-factor authentication:

1. In JAI, click **User Manager** > **Edit profile**.
2. Click **Two-Factor Authentication** tab.
3. Select an option from the **Authentication Method** drop-down list.

To disable an option, go to **Plugin Manager**, find the two-factor plugin and disable those that you do not want to use.

After you configure the two-factor authentication, the additional **Security Key** field is shown on API Portal login page for all users. Users can modify their settings in their profile page.

### Authentication methods

The following options are available from the **Authentication method** list:

* [Google Authenticator](https://en.wikipedia.org/wiki/Google_Authenticator): A software-based application provided by Google, which allows you to generate a six-digit security password that changes every 30 seconds. To use this option, you must download Google Authenticator on a smartphone, and configure it for each site which will be using it. You can enable two-factor authentication for the front-end, back-end, or for both. You can configure this in the **Two-Factor Authentication – Google Authenticator** plug-in.
* Yubikey: Allows you to use a Yubikey secure hardware token for two-factor authentication. In addition to the username and password, it also requires you to insert your Yubikey into the USB port of your computer (Click the **Secret Key** area of the site's login area and touch the **Yubikey's gold disk**). If you have an NFC-equipped Android smartphone you can just hold a compatible Yubikey token (Yubikey Neo) close to the NFC reader to copy the secret code to the device's clipboard. The secret code generated by your Yubikey is unique to your device, and it is constantly changing. You can enable two-factor authentication for the front-end, back-end, or for both. You can configure this in the plugin **Two-Factor Authentication – Yubikey** plug-in.

## Enable scanning of uploaded files

API Portal provides [ClamAV](https://www.clamav.net/documents/clam-antivirus-user-manual) anti-virus engine for scanning uploaded files for malicious content. ClamAV comes with *freshclam*, a tool that periodically checks for new database releases and keeps your database up to date.

Before enabling the scanning feature, you must install two packages in your machine:

* **clamav**: End-user tool for the Clam Antivirus scanner
* **clamd**: The Clam AntiVirus Daemon

Both packages are available in EPEL.

After installing the packages, you must start *ClamAV daemon*, the daemon that API Portal uses to scan the files.

After starting the daemon, you can enable scanning of uploaded files:

1. In JAI, click **Components** > **API Portal** > **Additional settings**.
2. Enable **Scan uploaded files** toggle.
3. Enter the ClamAV host and port.
4. Save the configuration.

If the configuration is incorrect or ClamAV daemon is not running, an error message will be shown.

{{< alert title="Note" color="primary" >}}If `PrivateTmp` flag is set to true for the Apache service and `/tmp` is set for `upload_tmp_dir` PHP configuration, ClamAV will not be able to scan uploaded files. In that case, you can change the value of `upload_tmp_dir` to `APIPORTAL_INSTALL_DIR/tmp`.{{< /alert >}}

## Do not allow web browsers to save login and password

When you log in to Joomla! Administrator Interface (JAI) do not allow the web browser to save or remember your login and password.

## Reject requests containing unexpected or missing content type headers

It is best practice to reject requests containing unexpected or missing content type headers with the HTTP response status `406 Unacceptable` or `415 Unsupported Media Type`.

The Content-Type header specifies what media type is being sent with the request. If the Content-Type header is missing, empty, or unexpected the server must refuse to serve the request with an appropriate response, as allowing the request might lead to Cross-Site Request Forgery (CSRF) or even remote code execution (RCE).

Add the configuration in your `.htaccess` file, virtual host file, or global web server configuration. The following code snippet gives an example for a server processing only `application/json` and `application/x-www-form-urlencoded` data.

```
# Check if the Content-Type header is missing or empty
RewriteCond %{HTTP:Content-Type} ^$
# AND the method type is POST, PUT or PATCH
RewriteCond %{REQUEST_METHOD} ^(POST|PUT|PATCH)
# Then redirect with response 415 Unsupported Media Type and stop processing other conditions
RewriteRule ^ - [R=415,L]
# OR Content-Type header is present
RewriteCond %{HTTP:Content-Type} !^$
# AND Content-Type value doesn't match one of the following, case-insensitive
RewriteCond %{HTTP:Content-Type} !^(application/json|application/x-www-form-urlencoded) [NC]
# Then redirect with response 415 Unsupported Media Type and stop processing other conditions
RewriteRule ^ - [R=415,L]
```

## Allow requests from only used HTTP methods

It is best practice to reject requests from HTTP methods that are not being used with the response `405 Method Not Allowed`. For example, allowing requests from the `TRACE` method might result in Cross-Site Tracing (XST) attacks. Similarly, allowing requests from `PUT` and `DELETE` methods might expose vulnerabilities to the file system.

`OPTIONS` method reports which HTTP methods are allowed on the web server, it is mainly used for debugging purposes. If you do not plan to run a diagnostic or debug the server, consider disabling this method.

`GET` and `POST` requests are mandatory for API Portal. You must also allow requests from the HTTP methods your listed APIs support, so users can send requests to them from the Try It page.

Add this configuration in your `.htaccess` or virtual host file. The following example allows only `GET`, `POST`, and `PUT` methods:

```
# Disable OPTIONS method
RewriteCond %{REQUEST_METHOD} ^OPTIONS
RewriteRule .* - [F]

# Disable TRACE method
TraceEnable off

# Enable GET, POST and PUT methods. Must be separated by a space character.
AllowMethods GET POST PUT
```

## Prevent host header attack

To prevent host header attacks, we recommend to whitelist `Host` and `X-Forwarded-Host` headers. When the values of the headers do not match the whitelist, the request is redirected to API Portal homepage. To whitelist headers, add the following configuration to your `.htaccess` or virtual host file. (Replace the placeholder URL with your domain):

```
RewriteCond %{HTTP_HOST} !^([a-zA-Z0-9-_]{1,20}.){0,3}APIPORTAL_DOMAIN$
RewriteRule ^(.*)$ https://APIPORTAL_YOUR_DOMAIN/ [R=301,L]

RewriteCond %{HTTP:X-Forwarded-Host} !^$
RewriteCond %{HTTP:X-Forwarded-Host} !^([a-zA-Z0-9-_]{1,20}.){0,3}APIPORTAL_DOMAIN$
RRewriteRule ^(.*)$ https://APIPORTAL_YOUR_DOMAIN/ [R=301,L]
```

## Protect the integrity of the logging system

To protect the integrity of the application generated logs, see [these security best practices for storing log files](/docs/apim_administration/apiportal_admin/apip_logging/).

## Utilize synchronized time source

We recommend that you synchronize API Portal server with an internal or external Network Time Protocol (NTP) server.

It is important to use unified and synchronized time source throughout the environment to correlate logs and data from different internal and external systems and preserve forensic quality of the logs. Accurate time is also essential when identifying and analyzing application events, including attacks.

## Detect and prevent the usage of automated tools or unusual behavior

Detecting abnormal behavior is very complex task and good prevention requires actions not only on the application layer but also concerns the firewall, network monitoring, proxy servers, and so on.

These are some general recommendations:

* IP throttling - limits the number of connections from a given IP address within a given time period (what is considered normal activity). You can achieve this in different ways:

    * Toolkits like `mod_security`. For more details, see [ModSecurity rules](https://modsecurity.org/rules.html).
    * Firewall configurations. For more details see the official documentation of your firewall.
* Google Analytics - has abnormal detection features. Very commonly used and reliable tool. For more information, see [Google Analytics Anomaly Detection](https://support.google.com/analytics/answer/7507748?hl=en).
* Log analysis tools - can be installed to act upon different logs. For example, see [Loggly Anomaly Detection](https://www.loggly.com/docs/anomaly-detection/).

## Define a restrictive Content Security Policy

Content Security Policy ([CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)) sets a policy that instructs the browser to only fetch resources, such as scripts, images, or objects, from the specified locations. After CSP is configured, a compliant browser loads resources from locations listed in the policy. CSP reduces the ability of an attacker to inject malicious content and helps to protect a web page from attacks like Cross-Site Scripting (XSS), dynamic code execution, and clickjacking.

CSP is enabled by default in API Portal. To change its configuration, in JAI, click **Extensions > Plugins > API Portal - System**. In the plugin settings you can edit, enable, or disable the policy. Because API Portal uses some inline scripts, you must use nonces to ensure that you are using API Portal securely. If you do not want to take advantage of the nonces you can remove them from the policy.

## Define retention periods for personal data

For compliance with General Data Protection (GDP), you must define retention periods within the design phase for all data fields taking into account the defined purpose. You must also include retention periods for backups.

If you are a small organization, you may not need a documented retention policy. However, if you do not have a retention policy, you must still regularly review the data you hold and delete or anonymise any data that you no longer need.

You must include your Data Protection Officer (DPO) or Legal department to define the retention periods as other laws may impact the retention requirements.

## Permanently delete unnecessary data

When the retention periods expire you must ensure that all of the data which is no longer needed is deleted. This may require automatic identification of the latest activities and a data deletion functionality or manual work.

## Where to go next

* For more information on the security features of API Management products and best practices for strengthening their security, see the [API Management Security Guide](/docs/apimgmt_security/).
* For privacy and personal data security recommendations, see [Manage privacy and personal data](/docs/apim_administration/apiportal_admin/manage_privacy_personal_data).
