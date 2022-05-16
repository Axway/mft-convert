---

    title:  Install Transfer CFT 
    linkTitle: Install Transfer CFT
    weight: 160

---
This section describes the steps to perform a Transfer CFT HP NonStop installation, as well as the automatic steps that occur when you run the installation procedure.

> **Note**
>
> To accommodate changing product versions, the convention &lt;version> may be used in place of the actual product version in examples and lists. You should replace &lt;placeholders>, or example values used for demonstrative purposes, with your actual values and target platform details.

## Upload the Transfer CFT archive

Upload the Transfer CFT package corresponding to the target platform to your machine. Perform this upload transfer in BINARY mode to ensure package integrity.

**Example**

```
Transfer_CFT_3.8_Install_hp_nonstop_oss-ia64-32_BN8580000.zip
```

Decompress the archive using the <span class="code">`unzip `</span>command.

**Example**

Depending on your installation, the screen message may differ slightly from the following example.

```
/home/cftuser: unzip Transfer_CFT_3.8_Install_hp_nonstop_oss-ia64-32_BN8
580000.zip
Archive:  Transfer_CFT_3.8_Install_hp_nonstop_oss-ia64-33_BN8580000.zip
   creating: Transfer_CFT_OtherUnixes_V3.8/
  inflating: Transfer_CFT_OtherUnixes_V3.8/TransferCFT_3.8_hp_nonstop_oss-ia
64-32.run
inflating: EULA.txt
inflating: EULA.html
```

In the example, Transfer CFT package is unzipped in the <span class="code">`Transfer_CFT_OtherUnixes_V3.8`</span> directory.

> **Note**
>
> Be certain to read the end-user license agreement.

## Add execution rights

Add execution rights to the <span class="code">`Transfer_CFT_<version>_<os>-<arch>-<xx>.run`</span> package.

Enter:

```
chmod u+x Transfer_CFT_<version>_<os>-<arch>-<xx>.run
```

## Start the installation

Execute the <span class="code">`install `</span>command to start the Transfer CFT installation procedure, replacing <span class="code">`<installation_directory>`</span> with the directory where you want to install Transfer CFT.

For a new installation, this directory should be empty or nonexistent. However, the installation directory can point to an existing installation in order to upgrade it. For more information on performing an upgrade, refer to <a href="" class="MCXref xref">Migrate or upgrade Transfer CFT</a>.

You can use the following additional parameters:

- <span class="code">`--cryptokey_password <password>`</span>: the <span class="code">`cftcrypt `</span>requires a password to generate an encryption key. You can either provide one using this parameter, or interactively enter it during the installation. Either way, the password is checked against the <span class="code">`cftcrypt `</span>password acceptance criteria, and the installation cannot complete unless a valid password is provided.
- `<guardian_installation_directory_prefix>` installs the Guardian specific files in its file system space. This parameter is *optional*. It is required only if you want to integrate Transfer CFT with Guardian procedures.
- <span class="code">`--post_install_script <fullpathtopinstscript>`</span> runs a shell script during the installation procedure after the product has been initialized.

Enter:

```
./Transfer_CFT_<version>_<os> <xx>.run install <installation_directory> [<guardian_installation_directory_prefix>] [--post_install_script <fullpathtopinstscript>] [--cryptokey_password <password>]
```

**Examples**

The following command installs Transfer CFT on the OSS directory /<span class="code">`home/cftuser/CFT38`</span>. Additionally, the Guardian components are installed using <span class="code">`/G/data14/cft38b`</span> (which is an equivalent of `$DATA14.CFT38B`) as a prefix.

```
/home/cftuser/Transfer_CFT_OtherUnixes_V3.8: ./ TransferCFT_3.8_hp_nonstop_oss-ia 64-32.run install /home/cftuser/CFT38 /G/data14/cft38b
```

The following command installs Transfer CFT on the OSS directory` /home/cftuser/CFT38`. Additionally, a post installation script `cft_postinst.sh` is run at the end of the installation process.

```
/home/cftuser/Transfer_CFT_OtherUnixes_V3.8: ./ TransferCFT_3.8_hp_nonstop_oss-ia 64-33.run install /home/cftuser/CFT38 --post_install_script ./cft_postinst.sh
```

The following command does the same as above, but additionally sets a password that is used to generate an encryption key so that you do not need to enter it interactively.

```
/home/cftuser/Transfer_CFT_OtherUnixes_V3.8: ./ TransferCFT_3.8_hp_nonstop_oss-x86-32.run install /home/cftuser/CFT38 --post_install_script ./cft_postinst.sh --cryptokey_password mypAsswOrd76\*
```

### Installation procedure results

The Transfer CFT installation procedure automatically performs the following:

- Extracts the Transfer CFT package.
- Creates a Transfer CFT runtime.
- Initializes the sample configuration.
- Creates the Transfer CFT database.
- Creates a default user for Transfer CFT Copilot:
    -   Login: **guest**
    -   Password: **guest**

<span id="Guardian"></span>

### Guardian files

If you opted to install the Guardian files, several files are created in the Guardian system space. The files' volume and subvolumes depend on the installation prefix that you provided.

For example, <span class="code">`/G/data14/cft38b`</span> creates files where the volume name is $DATA14, the subvolume names begin with cft38b, and that ends with the values described in the following table.


| Subvolume  | Description  |
| --- | --- |
| &lt;subvolume&gt;IE  | Contains the Transfer CFT samples.<br/> Some of these samples are copied in the user configuration volume &lt;subvolume&gt;UP, where they can be modified. |
| &lt;subvolume&gt;IF  | Contains the EMS dictionary and DDL template files.<br/> CFTPLATE contains the Transfer CFT templates to be concatenated with the system template for an EMS collector. See the XCFTDDL section in <a href="">Event messages</a>, which describes the DDL template. |
| &lt;subvolume&gt;IX  | Transfer CFT executables and procedures. |
| &lt;subvolume&gt;IP  | Program samples.  |
| &lt;subvolume&gt;IH  | Headers.  |
| <span id="subvolumeUD"></span>&lt;subvolume&gt;UD  | Default Transfer CFT subvolume.<br/> This is used when Transfer CFT needs to create a Guardian file. |
| &lt;subvolume&gt;UP  | Contains the procedures copied from the installation samples.  |


## Enter the Transfer CFT license key

After the installation completes, enter the Transfer CFT license key in the <span class="code">`<installation_directory>/runtime/conf/cft.key`</span> file.

<span id="Install"></span>

## Install the Guardian specific files

If you ran the installation procedure without providing the parameter <span class="code">`<guardian_installation_`</span>directory\_prefix>, the Guardian specific files were not installed. However, you can install these files later by calling the Guardian installation script.

1. Load the Transfer CFT profile:
1. Install the Guardian files:
1. ```
    cftginst.sh --help
    ```
1. Since installing the Guardian files modifies the Transfer CFT profile, you must reload the profile:
