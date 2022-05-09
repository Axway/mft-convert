{
    "title": "Migration prerequisites",
    "linkTitle": "Migration prerequisites",
    "weight": "240"
}This section describes two basic steps that you perform when migrating {{< TransferCFT/headerfootervariableshflongproductname  >}}.

Prerequisites
-------------

### Install Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}}

1. Perform a Transfer CFT installation, as described in the installation section.
1. After installing Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}}, update the to the most recent service pack.

### Load the environment

Before beginning the manual migration procedure, you must load the former (old) Transfer CFT environment. After loading the profile, you can execute commands from anywhere.

### Check TLS version

As of Transfer CFT 3.2.0 the use of cipher suites 59, 60, and 61 is restricted to TLS 1.2 exclusively. This means that if you are using one of these cipher suites, and you perform a migration from a version lower than 3.2.0, you must set the UCONF parameter `cft.ssl.version_max=tls_1.2`.
