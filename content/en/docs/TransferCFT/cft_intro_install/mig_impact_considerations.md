---
title: "Migration or upgrade impact and considerations "
linkTitle: "Migration/upgrade impact and considerations"
weight: 80
---You should be aware of the following features or parameters, which were modified since their inception, and how these changes may impact your upgrade or migration.

<table>
   <thead>
      <tr>
<th class="TableStyle-SynchTableStyle_interop-HeadE-Column1-Header1"><p>Key element</p>         </th>
<th class="TableStyle-SynchTableStyle_interop-HeadE-Column1-Header1">Originating versions         </th>
<th class="TableStyle-SynchTableStyle_interop-HeadE-Column1-Header1">Updated versions         </th>
<th class="TableStyle-SynchTableStyle_interop-HeadD-Column1-Header1">Description         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Discontinued PassPort PS         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.9 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.10 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>If you were using PassPort PS as your PKI type, we recommend the following steps prior to an upgrade or migration:</p>
<ol>
<li><p>Execute <code>CFTUTIL uconfunset id=pki.type</code> to set the type to the local PKI database.</p></li>
<li><p>Export all Transfer CFT certificates and keys from PassPort PS.</p></li>
<li><p>Import these keys and certificates in the PKI database.</p></li>
</ol>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Deprecated Habilitation Access Management         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.8 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.9 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">          </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">z/OS case sensitive file names         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.8 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.9 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>File names on USS systems are case sensitive and no longer need to be enclosed in quotes (“”).</p>
<ul>
<li>Prior to version 3.9, you required quotation marks to ensure case sensitivity.</li>
<li>As of version 3.9, case sensitivity is applied universally.</li>
</ul>
<p><strong>Example</strong></p>
<p>An example regression, if the USS the path is defined as follows:</p>
<table>
   <thead>
      <tr>
<th class="TableStyle-SynchTableStyle_interop-HeadE-Column1-Header1">          </th>
<th class="TableStyle-SynchTableStyle_interop-HeadE-Column1-Header1">USS path         </th>
<th class="TableStyle-SynchTableStyle_interop-HeadE-Column1-Header1">Send         </th>
<th class="TableStyle-SynchTableStyle_interop-HeadD-Column1-Header1">Consequence         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2" style="font-weight: bold">3.8         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">/DIR/FILE         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">FNAME="/dir/file"         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">file found         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2" style="font-weight: bold">3.9         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">/DIR/FILE         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">FNAME="/dir/file"         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">file not found         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyB-Column1-Body2" style="font-weight: bold">3.9         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyB-Column1-Body2">/DIR/FILE         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyB-Column1-Body2">FNAME="/DIR/FILE"         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyA-Column1-Body2">file found         </td>
      </tr>
   </tbody>
</table>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">API and Exits         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">all versions         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">to any version         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">You must recompile any API or Exit programs that are used by Transfer CFT.         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>Configuration cache feature</p>
<p>(cft.server.parm.cache_size)</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Lower than 3.8         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.8 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p><span id="parmcache"></span>The default value is now 5000 instead of zero, making the cache feature active by default.</p>
<p>This means that updates no longer occur dynamically; you can execute <code>RECONFIG </code>type<code>=PARMCACHE </code>or wait for a cache timeout as defined in <code>cft.server.parm.cache_timeout (60 seconds).</code></p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>cft.listcat_compat = No</p>
<p>cft.state_compat = No</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Lower than 3.8         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.8 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Modified the default value for the <code>cft.listcat_compat </code>(lstcompat) and <code>  cft.state_compat     </code>(stacompat) parameters from YES to NO.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Amazon S3         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Lower than 3.8         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.8 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>When using Amazon S3, the default setting FACTION=VERIFY is no longer ignored.</p>
<p>If you would like to continue to have the same behavior of overwriting the file, please use FACTION=DELETE. Note, though, that the file is not available during the transfer.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">CFTUIPREF         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Lower than 3.8         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.8 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">After an upgrade you may need to check user privileges for creating filters in the CFTUIPREF object.         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Copilot Java applet         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Lower than 3.8         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.8 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">The Copilot Java applet was removed from the product. Users are invited to use the Transfer CFT UI or Flow Manager for a graphical UI experience.         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">SQLite database         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.7 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.8 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>The CFTPARM object's PARTFNAM and PKIFNAME fields are obsolete for Windows, UNIX, and HP NonStop.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>FTYPE=T</p>
<p><em>Windows only</em></p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.2.4 to 3.7 without appropriate patch or SP*         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.8 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>On Windows systems, note the following difference when FTYPE=T.</p>
<ul>
<li>For versions 3.2.4 to 3.7 without the patch, an empty line terminated by a 1A character is transmitted.</li>
<li>Prior to 3.2.4 and for the versions with the SP or patch applied, an empty line terminated by a 1A character is not transmitted.</li>
</ul>
<p>*3.7 SP1 (patch), 3.3.2 SP8, 3.6 SP3, 3.8</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Visual C++ Redistributable Package for Visual Studio 2019
&lt;/td&gt;         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.6 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.7 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Transfer CFT on Windows requires the <strong>Visual C++ Redistributable Package for Visual Studio 2019</strong> for proper functioning. This provides the necessary library files (DLL) for Transfer CFT.</p>
<p>You must install <code>vcredist_x64.exe</code> prior to installing or upgrading Transfer CFT.</p>
<p><strong>Issue</strong></p>
<p>If you perform an upgrade without first installing the Redistributable package, the runtime is not imported and Transfer CFT will not operate correctly. The following information displays in the <code>&lt;installdir&gt;/install.log</code> file:</p>
<p><code>Script stderr:</code></p>
<p><code>child killed: unknown signal</code></p>
<p><code> </code></p>
<p><code>Fail to import RUNTIME data.</code></p>
<p><code>Problem running post-install step. Installation may not complete correctly</code></p>
<p><code>Fail to import RUNTIME data.</code></p>
<p><strong>Corrective action</strong></p>
<ol>
<li>Install the Redistributable package.</li>
<li>From the <code>cmd </code>console, load the profile.</li>
<li>Import the runtime data by running the import command to complete the upgrade.</li>
<li>Check that the script executed correctly.</li>
</ol>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">LISTPKI         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.6 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.7 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">To use the new LISTPKI format, copy the <code>dspcnf.xml</code> model file from <code>&lt;installdir&gt;/distrib/template/conf</code> to the <code>&lt;runtimedir&gt;/conf.</code>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">CFTACCNT         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.5 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.6 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Updated the documentation for the account file in v24 format. Please note the changes in field length as described in the CFTACCNT list.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">SORTBY         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.5 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.6 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Catalog records are no longer displayed by IDTU. To have the same display as in previous versions, use the SORTBY parameter as follows:<br />
<code>listcat sortby=idtu</code></p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">EBICS         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.5 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.6 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Use the Axway EBICS client. Please refer to the [EBICS client documentation](https://docs.axway.com/bundle/EBICSClient_10_allOS_en_HTML5/page/ebics_client_documentation_home.html) for product details.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>BUFSIZE</p>
<p>FBUFSIZE</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>3.3.2 and lower</p>
<p><em>Unix and IBM i only</em></p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>3.4, 3.6 SP2 and lower, 3.7 and 3.8</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>A BUFSIZE or FBUFSIZE value greater than 32 kiB may lead to Transfer CFT failing to exchange messages between CFTTPRO and CFTTFIL.
If you have set a value higher than 32 kiB, please decrease it to 32768.</p>
<blockquote>
<p><strong>Note</strong></p>
<p>As of 3.6 SP3, 3.8 SP1, and 3.9, the internal value limit is 32768.</p>
</blockquote>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">PKIFNAME         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.4 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.5 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">You can no longer reference a certificate with the PKIFNAME format (<code>CFTPARM:PKIFNAME=TXT://certificate</code>).
<p>Previously, when implementing an integrated
PKI, the PKIFNAME parameter could indicate a flat-file database (<code>PKIFNAME=TXT://certificate</code>). If you were using this kind of file and then migrate, you must manually import all certificates into the PKI database.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">CFTCRON         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Lower than 3.4         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.4 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">An upgrade from a version lower than Transfer CFT 3.4 to 3.4 or higher may fail due to an incorrect time syntax because the CFTCRON time syntax is checked when creating or editing a CFTCRON object.         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">PKIPASSW         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.3.2 or lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.4 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Removed the PKIPASSW parameter from PKI commands (still available for CFTPARM).</p>
<blockquote>
<p><strong>Note</strong></p>
<p>In earlier versions of Transfer CFT, the PKIPASSW parameter was used for encryption in the multiple PKI commands. This functionality is now replaced by the UCONF crypto.key_fname parameter.</p>
</blockquote>
<p><strong><strong>Impact</strong></strong></p>
<p>If you are using PKIEXT to export keys during a manual migration, you must use the same PKIPASSW (CFTPARM object) as was originally used to import the key. Using the same logic, to re-import a key that you extracted using PKIEXT, you require the same CFTPARM [PKIPASSW](../../c_intro_userinterfaces/command_summary/parameter_intro/pkipassw).</p>
<p>For information on exporting keys, please refer to [Using PKIEXT](../../transport_security_start_here/certificates/pkiutil_cli_intro/pkiext).</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">32-bit releases         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.3.2 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.4 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">End of 32-bit version deliveries.         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Some default values         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.3.2 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.4 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Updated default values of the following parameters to optimize and standardize among platforms.</p>
<table>
<thead data-xmlns="">
      <tr>
<th class="TableStyle-SynchTableStyle_interop-HeadE-Column1-Header1">Object         </th>
<th class="TableStyle-SynchTableStyle_interop-HeadE-Column1-Header1">Parameter         </th>
<th class="TableStyle-SynchTableStyle_interop-HeadE-Column1-Header1">Old default         </th>
<th class="TableStyle-SynchTableStyle_interop-HeadD-Column1-Header1">New default         </th>
      </tr>
   </thead>
<tbody data-xmlns="">
      <tr>
         <td rowspan="7" class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p><strong>CFTPARM</strong></p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>
<p> </p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>MAXTRANS</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>128 (Win), 256
(os400, unix, vms), 990 (z/OS)</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>256</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>MAXTASK</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>1 (Win), 16
(os400, unix, vms), 400 (z/OS)</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>8</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>TRANTASK</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>14 (z/OS), 16
(os400, unix, vms), 128 (win)</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>3</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>WAITTASK</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>1441</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>10</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>SSLMTASK</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>1 (Win), 16
(os400, unix, vms), 64 (z/OS)</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>8</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>SSLTTASK</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>14 (z/OS), 16
(os400, unix, vms), 128 (win)</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>3</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>SSLWTASK</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>1441</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>10</p>         </td>
      </tr>
      <tr>
         <td rowspan="2" class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2" style="font-weight: bold"><p>CFTNET</p>
<p> </p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2" style="font-weight: bold"><p>type</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2" style="font-weight: bold"><p>x25</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2" style="font-weight: bold"><p>TCP</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>maxcnx</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>32</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>384</p>         </td>
      </tr>
      <tr>
         <td rowspan="16" class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2" style="font-weight: bold"><p>CFTPROT type=PeSIT prof=ANY</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>concat</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>no</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>yes</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>multart</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>no</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>yes</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>segment</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>no</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>yes</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>rpacing</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>36</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>32767</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>spacing</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>36</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>32767</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>rrusize</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>4056</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>32750</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>srusize</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>4056</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>32750</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>disctc</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>90</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>60</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>disctd</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>120</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>10</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>disctr</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>45</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>45</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>discts</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>165</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>60</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>rchkw</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>2</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>3</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>schkw</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>2</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>3</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>rcomp</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>10</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>0</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>scomp</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>10</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>0</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">sserv         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">PESIT         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">GSIT         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2" data-xmlns=""><strong>CFTPROT type=ODETTE</strong>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">tcp         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">CFT         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">OFTP         </td>
      </tr>
      <tr>
         <td rowspan="4" class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p><strong>CFTTCP</strong></p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>retryw</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>7</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>1</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>retryn</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>6</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>4</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>retrym</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>12</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>12</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyB-Column1-Body2"><p>cnxinout</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyB-Column1-Body2"><p>2</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyA-Column1-Body2"><p>4</p>         </td>
      </tr>
   </tbody>
</table>
<p><strong>Impact</strong></p>
<p>Check the use in your flows and modify according.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">cft.server.processing_scripts_variables_blacklist         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.3.2 SP3 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.3.2 SP4 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2">POSIX Regular Extended expression that defines forbidden characters.         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">TLS         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.2.x and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>not applicable</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>When migrating to 3.2.x or higher, SSL transfers may fail with a DIAGP e105s86 or e75s89 when performing transfers with the versions listed below (with the error occurring on the remote {{< TransferCFT/suitevariablesTransferCFTName  >}}).</p>
<p>Affected versions:</p>
<ul>
<li>All 3.1.3 SP7 and lower</li>
<li>All 3.0.1 SP3 and lower</li>
</ul>
<p>On even older versions, we recommend setting the CFTPROT:CONCAT parameter to No.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">CA certificate chains         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.1.3 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.2.2 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>In {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.1.3 and lower, you can perform a SSL transfer even if the certificate chain is not complete (not signed by a ROOT CA).</p>
<p><strong>Impact</strong></p>
<p>In {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.2.2 and higher, the certificate chain must be complete for a transfer to succeed.</p>
<p>For more information, see [Unknown CA leads to a failed certificate verification](../../troubleshoot_intro/admin_troubleshooting_server/troubleshoot_security#Unknown)</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">PKIPASSW         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.1.3 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.3.2 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>When upgrading from 3.1.3 to 3.3.2, first check that the PKIPASSW length value is not greater than 8 characters.</p>
<p>If the value is 8 or less, you can proceed with the upgrade.</p>
<p>If the PKIPASSW value in the CFTPARM command is greater than 8 characters, perform the steps in the solution below.</p>
<p><strong><strong>Solution</strong></strong></p>
<p>Prior to migration you  must truncate the password on the Transfer CFT 3.1.3:</p>
<ol>
<li>Export the CFTPARM.<br />
<code>CFTUTIL cftext type=parm, fout=file_parm.out</code></li>
<li>Modify the PKIPASSW in the file. For example, if the old value was <code>PKIPASSW=12345678910</code>, replace it with <code>PKIPASSW=12345678.</code></li>
<li>Reimport:<br />
<code>CFTUTIL config type=input,fname=file_parm.out</code></li>
<li>Continue the Transfer CFT 3.3.2 upgrade process.</li>
</ol>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Copilot client         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>3.1.3 or lower</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.2.2 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>The Copilot application changed from a Java applet to a Java Web Start program.</p>
<p><strong><strong>Impact</strong></strong></p>
<p>Copilot requires Java 7 or higher.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">ROOTCID=NONE         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.1.3         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.2.2 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Non authentication method was available in 3.1.3 and lower (anonymous TLS connection).</p>
<p><strong><strong>Impact</strong></strong></p>
<p>This support has been removed in {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.2.2 and higher.
You must update the ROOTCID parameter.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">TLS         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.1.3 or lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.2.2 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>To comply with security standards, as of Transfer CFT version 3.2.2 the use of the cipher suites 59, 60, and 61 is restricted to TLS 1.2 exclusively.</p>
<p><strong><strong>Impact</strong></strong></p>
<p>This means that if some of your partners use a version of Transfer CFT lower than 3.2.2 that does not support TLS 1.2, and you are using ciphers 59, 60 and 61, which requires TLS 1.2 in version 3.2.2 and higher, you must add another cipher in the cipher list and remove ciphers 59, 60, 61 from the partner's cipher list.</p>
<blockquote>
<p><strong>Note</strong></p>
<p>You do not have to remove ciphers 59, 60, 61 in the partner cipher list if you apply the Transfer CFT patch 3.0.1 SP11.</p>
</blockquote>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2"><p>Symbolic variable prefix</p>
<p><em>HP NonStop only</em></p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">2.3.2 and lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.3.2 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>The symbolic variable prefix has changed from the circumflex (^) to an ampersand (&amp;).</p>
<p><strong><strong>Example</strong></strong></p>
<p><code>FNAME=^NFNAME</code> becomes <code>FNAME=&amp;NFNAME</code></p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Rotate the log         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.0.1 or lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.1.3 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Changed the switch log feature behavior.</p>
<p>In version 3.0.1 or lower, there were two files that automatically alternated.</p>
<p><strong><strong>Impact</strong></strong></p>
<p>In version 3.1.3 and higher if you want to continue this functionality, you must set the alternate log file's uconf value <code>cft.cftlog.afname</code> to the alternate file path (for example, <code>$CFTRUNTIME/log/cftloga</code>).</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">Demo certificates         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.0.1 or lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyE-Column1-Body2">3.1.2 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyD-Column1-Body2"><p>Axway no longer delivers the template certificates used in the Transfer CFT SSL.</p>
<p><strong><strong>Impact</strong></strong></p>
<p>If you were using the demo certificates, import your proper certificates and replace in the PKI database as the Demo certificates are expired.</p>         </td>
      </tr>
      <tr>
         <td class="TableStyle-SynchTableStyle_interop-BodyB-Column1-Body2"><p>CFTPARM </p>
<p>key parameter</p>         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyB-Column1-Body2">2.7.1 or lower         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyB-Column1-Body2">3.0.1 and higher         </td>
         <td class="TableStyle-SynchTableStyle_interop-BodyA-Column1-Body2">If you had the CFTPARM key parameter set directly to a value, you must modify this so that key parameter points to an indirection file containing the license key.         </td>
      </tr>
   </tbody>
</table>

 
