{
"title": "System requirements",
  "linkTitle": "System requirements",
  "weight": "4",
  "date": "2019-10-02",
  "description": "Supported platforms and other system requirements for API Gateway, and specific requirements for API Gateway components."
}

## Operating systems and hardware

This section describes the operating system requirements for API Gateway.

### Linux

API Gateway supports the following software versions:

* CentOS 6.x, 7.x, 8.x (CentOS 8.x is supported from [API Gateway May 20](/docs/apim_relnotes/20200530_apimgr_relnotes/) update onward.)
* Oracle Linux 6.x, 7.x
* Red Hat Enterprise Linux 6.x, 7.x, 8.x (RHEL 8.x is supported from [API Gateway May 20](/docs/apim_relnotes/20200530_apimgr_relnotes/) update onward.)
* SUSE Linux Enterprise Server 11.x, 12.x

API Gateway supports the following hardware:

* 64-bit Linux running on 64-bit hardware
* Intel Core or AMD Opteron at 2Ghz with Dual Core or faster

{{< alert title="Note" color="primary" >}}When new Linux kernels and distributions are released, Axway modifies and tests its products for stability and reliability on these platforms.
Axway makes every effort to add support for new kernels and distributions in a timely manner. However, until a kernel or distribution is added to this list, its use with API Gateway is not supported. Axway endeavors to support any generally popular Linux distribution on a release that the vendor still supports.{{< /alert >}}

Your Linux system must have the `LANG` environment variable set. If this variable is not configured correctly, your system might have issues handling Unicode characters in file names. A full installation of Linux should configure this for you automatically. If you are running the API Gateway in a Docker image that you have built, set this variable in your Dockerfile as follows:

```
ENV LANG=en_US.UTF-8
```

This variable is automatically set for you in EMT mode, but it can be overridden by directly modifying the EMT API Gateway Dockerfiles or setting an environment variable at container runtime. For more information, see [Best practices for running API management in Docker containers](/docs/apim_howto_guides/apigw_in_containers/#setting-locale-environment-variables).  

In CentOS 8.x and Red Hat Enterprise Linux 8.x, the GNU C Library (glibc) no longer includes `libnsl` and `glibc-langpack-en` libraries. However, those were included in previous releases and are required by the API Gateway. To install the missing libraries, run the following command:

```
sudo yum install libnsl glibc-langpack-en
```

### Windows

Windows is supported only for a limited set of developer tools, see [Install developer tools on Windows](/docs/apim_installation/apigtw_install/install_dev_tools/). API Gateway and API Manager do not support Windows.

This section addresses Policy Studio, Configuration Studio, and Package and Deployment tools only.

Developer tools supports the following software versions:

* Windows 10
* Windows 8.1

Developer tools supports the following hardware:

* 32-bit Windows on both 32-bit hardware and 64-bit hardware
* Intel Core or AMD Opteron at 2Ghz with Dual Core or faster

### Disk space and RAM requirements

The disk space and RAM requirements for Linux platforms are:

* Disk space: minimum 4 GB, 50 GB recommended
* Physical memory (RAM): minimum 8 GB

The disk space and RAM requirements for the developer tools on Windows platforms are:

* Disk space: minimum 2 GB
* Physical memory (RAM): minimum 4 GB

There are also specific requirements for the `/tmp` directory:

* Minimum 500 MB available in the `/tmp` directory and writable permissions on the `/tmp`, `/var/tmp`, and `/usr/tmp` directories.
* `noexec` must not be set on `/tmp`. If `noexec` is set, you must remount `/tmp` with `noexec` disabled or follow the additional steps detailed in [Linux tmp directory mounted with noexec](/docs/apim_installation/apigtw_install/system_requirements#linux-tmp-directory-mounted-with-noexec).

## Databases

This section describes the supported database versions.

### Relational databases

API Gateway and API Manager support the following relational databases to store metrics data:

* MySQL Server 5.7, 8.0
* MariaDB 10.2, 10.5
* Microsoft SQL Server 2016, 2017, and 2019
* Oracle 12.2, 18c, and 19c
* IBM DB2 10.5

For more details, see [Install and configure a metrics database](/docs/apim_installation/apigtw_install/metrics_db_install).

### Apache Cassandra

API Gateway and API Manager support Apache Cassandra version 2.2.12 for internal data storage. For more details, see [Install an Apache Cassandra database](/docs/apim_installation/apigtw_install/cassandra_install).

## Web browsers

API Gateway Manager and other browser-based client components support the following browsers:

* Internet Explorer 11
* Firefox 13.0 or higher
* Safari 5.1.7 or higher
* Google Chrome 19 or higher
* Microsoft Edge (on Windows 10 only)

## Thick client platforms

Policy Studio has the following additional requirements on Linux:

* X-Windows environment
* GTK+ 2

## Docker containers

API Gateway and API Manager support elastic topology in Docker deployments. For details of Docker support, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

{{< alert title="Note" color="primary" >}}If you are using API Manager monitoring, you also require a shared file system between your API Gateway instances and Admin Node Manager. This is required for processing of transaction event logs and writing API metrics to a database.{{< /alert >}}

## Specific component requirements

This section describes requirements for specific API Gateway components.

* **Policy Studio** is a thick client and supports the platforms described in [Thick client platforms](#thick-client-platforms).
* **API Gateway Manager** is a web-based client and supports the web browsers listed in [Web browsers](#web-browsers).
* **API Gateway Analytics** is a server component that has the same [operating system and hardware](#operating-systems-and-hardware) requirements as API Gateway. Its browser-based client component supports the same [browsers](#web-browsers) as API Gateway Manager. API Gateway Analytics also requires a [database](#databases), and the following packages:
    * libXext
    * libXrender
    * xorg-x11-fonts-Type1
    * xorg-x11-fonts-75dpi
* **API Manager** is a browser-based client and supports the same [browsers](#web-browsers) as API Gateway Manager.

## Default ports

This section describes the default ports used by API Gateway components.

### API Gateway

The default ports used by API Gateway are as follows:

* **Traffic port**: `8080` (between clients and API Gateway)
* **Management port**: `8085` (between API Gateway and Admin Node Manager)

### Admin Node Manager

The default port used by the Admin Node Manager for monitoring and management of API Gateway instances is `8090`.

### Policy Studio

The default URL address used by the Policy Studio tool to connect to the Admin Node Manager is as follows:

```
https://localhost:8090/api
```

### API Gateway Manager

The default URL address used by the API Gateway Manager web console to connect to the Admin Node Manager is as follows:

```
https://localhost:8090/
```

### API Manager

The default URL address used by the API Manager web console for API management is as follows:

```
https://localhost:8075/
```

### API Gateway Analytics

The default port used by API Gateway Analytics for reporting, monitoring, and management is `8040` . The default URL address used by the API Gateway Analytics web console is as follows:

```
http://localhost:8040/
```

## Software and license keys

Axway products are delivered electronically from [Axway Support](https://support.axway.com/). A welcome email notifies you that your products are ready for download.

When you are ready, perform the following tasks:

1. Check your authorization.
2. Check the hardware and system requirements.
3. Obtain license keys.
4. Download the installation setup file from [Axway Support](https://support.axway.com/).
5. Install products.

### Check your authorization

Verify that you can log in to [Axway Support](https://support.axway.com/). If you do not have an account, follow the instructions in your welcome email.

Log in to download or access:

* The product installation package
* Your product license key
* Product documentation
* Product updates, including patches and service packs
* Product announcements
* The support case center, to open a new case or to track opened cases

You can also access other resources, such as articles in the Knowledge Base, the Axway User Forum, and documentation for all Axway products.

### License keys

API Gateway requires the following license keys.

**Axway license file**:

You must have a valid Axway license file to install the following API Gateway components:

* API Gateway Server
* API Gateway Analytics
* API Manager

You can obtain an evaluation trial license to enable you to evaluate the API Gateway features. However, you must have a full license to enable all API Gateway features for use in a non-evaluation environment (for example, development, testing, or production). To obtain an evaluation trial license or a full license, contact your Axway Account Manager.

You can install an Admin Node Manager in isolation without an API Gateway license. For more information, see [Install the Admin Node Manager](/docs/apim_installation/apigtw_install/install_node_manager/).

**McAfee license file**:

You must have a valid McAfee license file to use the **McAfee Anti-Virus** filter.

**FIPS-compliant mode license file**:

You must have a valid Axway FIPS-compliant mode license file to run API Gateway in FIPS-compliant mode.

### Multiple installations

API Gateway requires a minimum of two installations for high availability (HA). Make sure that you obtain license keys for all of the API Gateway instances that you are installing.

## Additional prerequisites

This section lists additional prerequisites for installing API Gateway.

On Linux, you must ensure that the installation executable has the appropriate permissions in your environment. For example, you can use the `chmod` command to update the file permissions.

### Linux tmp directory mounted with noexec

If your Linux system has the `/tmp` directory mounted with `noexec`, you must complete some additional steps before installing or running API Gateway.

**Installation**:

When installing API Gateway, do not install the QuickStart tutorial:

* When running the installer in GUI mode, you must select the **Custom** setup type and deselect the QuickStart tutorial component. For more information, see [Installation options](/docs/apim_installation/apigtw_install/installation/#installation-options).
* When running the installer in unattended mode, you must use the `--setup_type advanced` option and specify `qstart` to the `--disable-components` option. For more information, see [Unattended installation](/docs/apim_installation/apigtw_install/installation_unattended/).

{{< alert title="Note" color="primary" >}}Do not install the [QuickStart tutorial](/docs/apim_installation/apigtw_install/install_quickstart_tutorial/) as this option starts Apache Cassandra, the API Gateway server and the Node Manager when installation completes, and in a system with `/tmp` mounted as `noexec` you must make some changes before starting these components.{{< /alert >}}

**Post-installation**:

After completing the installation and before starting the services:

1. Create a new temporary directory that has `exec` privileges (for example, `/opt/Axway-7.7/tmp`).
2. If you installed Cassandra during API Gateway installation, edit the file `CASSANDRA_INSTALL_DIR/conf/cassandra-env.sh` and add the following line:

   ```
   JVM_OPTS="$JVM_OPTS -Djava.io.tmpdir=<TheNewTmpDir>"
   ```
3. Create or edit the file `VDISTDIR/apigateway/conf/jvm.xml`, and add the following:

   ```xml
   <ConfigurationFragment>
       <VMArg name="-Djava.io.tmpdir=<TheNewTmpDir>"/>
   </ConfigurationFragment>
   ```

### Service packs

Service packs for API Gateway are available from [Axway Support](https://support.axway.com/). If any service packs are available for API Gateway 7.7, download and apply them when the installation completes.

For more information on applying a service pack, see [Update API Gateway](/docs/apim_installation/apigtw_install/install_service_packs).

### Certificates

API Gateway uses Secure Sockets Layer (SSL) for communications between all processes in a domain (for example, internal management traffic between the Admin Node Manager and API Gateway instances).

Certificates are not required during installation; however, certificates will be required after installation to secure API Gateway domains. For more information on configuring and securing API Gateway domains, see the [API Gateway Administrator Guide](/docs/apim_administration/apigtw_admin/).
