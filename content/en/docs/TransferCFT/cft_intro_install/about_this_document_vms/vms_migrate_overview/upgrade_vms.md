{
    "title": "Manually upgrade Transfer CFT",
    "linkTitle": "Manually upgrade",
    "weight": "230"
}This section explains how to upgrade an existing Transfer CFT from 2.7.1, 3.0.1, 3.1.3 to a Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}}.

Before you start
----------------

Before beginning the upgrade procedure, you should:

- Check that you have the Transfer CFT Upgrade Pack available for download from Axway Sphere at [support.axway.com](https://support.axway.com/).
- Stop the Transfer CFT server and the Transfer CFT Copilot (UI) server. Enter:

```
cftstop
copstop -f
```

Display product details
-----------------------

You can use the display command to check the version or product details prior to upgrading.

```
CFTUTIL about
```

Manually upgrade a single installation
--------------------------------------

The manual upgrade procedure is similar to the migration procedure, and consists of:<span id="step3_single"></span>

1. Load the former Transfer CFT CFT 2.7.1, 3.0.1, or 3.1.3 environment.
1. Stop Transfer CFT.
1. Create a temporary directory for your exported configuration:

```
create/dir DKB200:[CFT_EXPORT]
SET DEF DKB200:[CFT_EXPORT]
```

1. Export your configuration.

    -   Export your static configuration objects using the command CFTUTIL CFTEXT:

    ```
    CFTUTIL CFTEXT type=all, fout=cft-extract.conf
    ```

    -   Export your PKI certificates using the PKIUTIL PKIEXT command. Enter:

    ```
    PKIUTIL PKIEXT fout=pki-extract.conf
    ```

    -   Export the catalog using the CFTMI command. Enter:

    ```
    CFTMI MIGR type=CAT, direct=FROMCAT, ifname=CFTCATA, ofname=catalog_output.xml
    ```

    -   Export the communication media file using the CFTMI command:

    ```
    CFTMI MIGR type=COM, direct=FROMCOM, ifname=CFTCOM, ofname=com_output.xml
    ```

    -   You must also export procedures that are specific to your production, sample APIs, exits, execs, etc.
        -   Copy your API, and exit sources.
        -   Copy your exec and specific scripts.

    > **Note**
    >
    > Note You must recompile these after upgrading.

1. Rename your Transfer CFT directory environment.
    &lt;ul&gt;&lt;li&gt;Rename the INSTALL directory:&lt;/li&gt;&lt;/ul&gt;
    ```
    SH LOG CFTDIRINSTALL
    RENAME DKB200:[CFTREF.CFT322.CFT]HOME.DIR
    DKB200:[CFTREF.CFT322.CFT]HOME_SAVE.DIR
    ```
    -   Rename the RUNTIME directory :

    ```
    SH LOG CFTDIRRUNTIME
    RENAME DKB200:[CFTREF.CFT322.CFT]RUNTIME.DIR
    DKB200:[CFTREF.CFT322.CFT]RUNTIME_SAVE.DIR
    ```
1. You can now install the new Transfer CFT version. See [Installing Transfer CFT on a single host](../../c_cft_introduction_vms/installation/t_install_single_host).
1. Import the configuration.
    -   Import the configuration that you saved previously in the temporary directory created in [Step 3](#step3_single).

    ```
    SET DEF DKB200:[CFT_EXPORT]
    ```
    -   Import your static configuration objects using the command:

    ```
    cftinit cft-extract.conf
    ```
    -   Import your PKI certificates using the PKIUTIL command. Enter:

    ```
    PKIUTIL PKIFILE fname=CFTPKU, mode='CREATE’
    PKIUTIL @pki-extract.conf
    ```
    -   Import the catalog using the CFTMI command. Enter:

    ```
    CFTM MIGR type=CAT, direct=TOCAT, ifname=catalog_output.xml,
    ofname=CFTCATA
    ```
    -   Import the communication media file using the CFTMI command:

    ```
    CFTMI MIGR type=COM, direct=TOCOM, ifname=com_ouput.xml,
    ofname=CFTCOM
    ```
    -   Import specific procedures to your production, for example APIs, EXITs, execs, etc.:
        -   Copy your API and EXIT sources.
        -   Copy your exec and specific scripts.

### Check the new version

To check the Transfer CFT version, as well as the license key and system information, enter:

```
CFTUTIL about
```
<span id="Upgradin"></span>

Manually upgrade a Transfer CFT 3.0.1 or 3.1.3 multi-node installation
----------------------------------------------------------------------

The multi-node procedure is similar to the non-multinode procedure. However, when exporting CAT and COM, you must export your configuration for each node. <span id="temp_dir_step3"></span>

1. Load the former Transfer CFT 2.7.1, 3.0.1, or 3.1.3 environment.
1. Stop Transfer CFT OpenVMS.
1. Create a temporary directory for your exported configuration:

```
create/dir DKB200:[CFT_EXPORT]
SET DEF DKB200:[CFT_EXPORT]
```

1. Export your configuration.

    -   Export your static configuration objects using the command CFTUTIL CFTEXT:

    ```
    CFTUTIL CFTEXT type=all, fout=cft-extract.conf
    ```

    -   Export your PKI certificates using the PKIUTIL PKIEXT command. Enter:

    ```
    PKIUTIL PKIEXT fout=pki-extract.conf
    ```

    -   Export the catalog using the CFTMI command. Enter:

    ```
    CFTMI MIGR type=CAT, direct=FROMCAT, ifname=<catalog_filename_
    former_cft_for_node_<node>>, ofname=catalog_output_<node>.xml
    ```

    -   Export the communication media file using the CFTMI command:
        -   For each communication media file, enter:
        -   For each node, enter:

    <!-- -->

    -   You must also export procedures that are specific to your production, for example APIs, exits, execs, etc.
        -   Copy your API and exit sources.
        -   Copy your exec and specific scripts.

    > **Note**
    >
    > You must recompile these after upgrading.

1. Rename your Transfer CFT directory environment for each hostname.
    &lt;ul&gt;&lt;li&gt;For the first hostname (n°&lt;i&gt;1&lt;/i&gt;): &lt;ul&gt;&lt;li&gt;Rename the INSTALL directory:&lt;/li&gt;
    ```
    SH LOG CFTDIRINSTALL
    RENAME DKB200:[HOST1.CFT322.CFT]HOME.DIR DKB200:[
    HOST1.CFT322.CFT]HOME_SAVE.DIR
    ```
1. Rename the RUNTIME directory:

- For each additional hostname (n°*x*):
    -   Rename the INSTALL directory:

You can now install the new Transfer CFT version. See [Installing Transfer CFT on multiple hosts](../../c_cft_introduction_vms/t_install_multiple_host).

Import the configuration that you saved previously in the temporary directory created in [Step 3](#temp_dir_step3).

```
SET DEF DKB200:[CFT_EXPORT]
```

- Import the static configuration objects using the command:

```
PKIUTIL PKIFILE fname=CFTPKU, mode='CREATE’
PKIUTIL @pki-extract.conf
```

- Import the PKI certificates using the PKIUTIL command:

```
PKIUTIL PKIFILE fname=CFTPKU, mode='CREATE’
PKIUTIL @pki-extract.conf
```

- Import the catalog using the CFTMI command:

```
CFTM MIGR type=CAT, direct=TOCAT, ifname=catalog_output_
<node>.xml, ofname=<catalog_filename_new_cft><node>
```

- Import the communication media file using the CFTMI command:
    -   For each communication media file, enter:
    -   For each node, enter:
- Import specific procedures to your production, for example APIs, EXITs, execs, etc.:
    &lt;ul style="list-style-type: circle;"&gt;&lt;li&gt;Copy your API and EXIT sources.&lt;/li&gt;&lt;/ul&gt;&lt;ul style="list-style-type: circle;"&gt;&lt;li&gt;Copy your exec and specific scripts.&lt;/li&gt;&lt;/ul&gt;&lt;blockquote&gt;&lt;html&gt;&lt;body&gt;&lt;p&gt;&lt;b&gt;Note&lt;/b&gt; &lt;/p&gt;&lt;/body&gt;&lt;/html&gt;You must recompile these after upgrading.&lt;/blockquote&gt;&lt;/li&gt;

### Check the new version

To check the Transfer CFT version, as well as the license key and system information, enter the command:

```
CFTUTIL about
```
