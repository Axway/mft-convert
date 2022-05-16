---

    title: recnb
    linkTitle: recnb
    weight: 2830

---
<span id="recnb"></span>

### {{< TransferCFT/SystemTitle  >}}

#### CFTFILE, TYPE=COM or TYPE=CAT

**TYPE=COM**

**\[RECNB = { 1... 100000 } \]**

Number of records in the file. The default value is the value set in the UCONF <span class="code">`cft.cftcom.default_size`</span> parameter.

****TYPE=CAT****

**\[RECNB =  { 1...10000000 } \]**

Number of records in the file. The default value is the value set in the UCONF <span class="code">`cft.cftcat.default_size`</span> parameter. This value corresponds to the maximum
number of transfer "entries" simultaneously present in the catalog.

It is highly recommended that you either customize the RH, SH, RT, ST, etc. parameter values in the CFTCAT object or customize the <span class="code">`cft.purge.*`</span> uconf values to prevent the file from overloading, as this would result in new transfer requests being refused to be taken into account.

When the catalog is full, the following error message is displayed: <span class="code">`CFTC29W Catalog Alert fill threshold reached: level=100% ID=CAT0`</span>

Please see the <a href="../../../../admin_intro/admin_monitoring_intro/housekeeping_catalog" class="MCXref xref">Housekeeping for catalog and output files</a> for best practices, or the <a href="../../../../troubleshoot_intro/admin_troubleshooting_server/admin_troubleshooting_runtime/troubleshoot_catalog" class="MCXref xref">Troubleshoot the catalog</a> page for solutions if the catalog is full and Transfer CFT has stopped.

#### RECONFIG

****TYPE=CAT****

**\[RECNB =  {
0... 10000000 } \]**

Number of records in the file. When TYPE = CAT, this value is mandatory.

[Return to Command index](../../)
