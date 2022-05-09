{
"title": "Manage API firewalling",
"linkTitle": "Manage API firewalling",
"weight":"80",
"date": "2019-10-14",
"description": "Enable API firewalling on an API Gateway interface in Policy Studio, and monitor API firewalling in API Gateway Manager."
}

API Gateway provides API firewalling capabilities by embedding Apache ModSecurity. This is a toolkit for real-time HTTP traffic monitoring, logging, and access control. This helps companies to mitigate application-level threats to their APIs. For example, this includes cross-site scripting, SQL injection, command injection, cross-site request forgery, and many others.

You can configure the embedded ModSecurity engine to protect API Gateway HTTP traffic against threats and monitor reported exceptions.

For more details on ModSecurity, see [Apache ModSecurity documentation](http://www.modsecurity.org/).

## Configure API firewalling

ModSecurity provides very little protection on its own. However, you can configure the required protection by configuring The ModSecurity rules engine with a threat protection profile. Protecting against specific threats requires specific rules, and different vendors provide rules for specific threat protection capabilities.

The Open Web Application Security Project (OWASP) [ModSecurity Core Rule Set (CRS) project](https://modsecurity.org/crs/) provides a popular rule set. You can use CRS version 2.x or 3.x. For more details on how to configure CRS version 3.x, see [Use OWASP ModSecurity CRS version 3.x](#use-owasp-modsecurity-crs-version-3-x). For more details on OWASP, see [OWASP website](https://www.owasp.org/).

For an example of how to write security rules yourself, see [How To Write A WAF Rule - Modsecurity Rule Writing](https://support.kemptechnologies.com/hc/en-us/articles/209635223-How-to-write-a-WAF-rule-Modsecurity-Rule-Writing).

### Enable threat protection

By default, the embedded ModSecurity engine is disabled. To enable ModSecurity on an API Gateway interface, perform the following steps:

1. In the Policy Studio node tree, click **Environment Configuration > Listeners**, and select the interface you want to enable (for example, **API Gateway > Default Services > Ports**).
2. Right-click the HTTP or HTTPS interface in the window on the right, and select **Edit**.
3. Go to the **Advanced** tab.
4. Under **Threat Protection Settings**, browse to the **Threat Protection Profile** you want to use to protect this interface with ModSecurity rules. For example:

    ![Enable threat protection](/Images/APIGateway/admin_waf_enable.png)

    When a profile is selected, all traffic is processed by the ModSecurity engine, and threats are rejected based on the selected security rules.

#### Configure a threat protection profile

If no threat protection profiles have been configured, perform the following:

1. In the **Select WAF Profile** dialog, right-click the **Threat Protection Profiles**, and select **Add a Threat Protection Profile**.
2. Enter a profile name, and configure the following settings:

    **Configuration directory**: Enter the name of the directory that stores the threat protection configuration file. When threat protection has been enabled, the embedded ModSecurity engine looks for threat protection rules configuration in this directory. API Gateway uses the OWASP ModSecurity CRS directory structure. The default is `${environment.VDISTDIR}/system/conf/threat-protection/default`.

    **Configuration file**: Enter the threat protection configuration file name. The default value is `modsecurity.conf`. This file contains the engine global settings. See [Configure modsecurity.conf file](#configure-modsecurity-conf-file).

    **Rules directory**: Enter the name of the subdirectory that stores the threat protection rules. When you download or create ModSecurity security rules, you must put them in this subdirectory. The embedded ModSecurity engine loads all `.conf` files in this directory. The default is `${environment.VDISTDIR}/system/conf/threat-protection/default/activated_rules`.

    **Alert policy**: Select an API Gateway policy you have configured that is executed when a threat protection rule is triggered and `SecRuleEngine` is set to `On`. The policy can, for example, include an Alert filter to send alert notifications to monitoring systems, or call an API. This setting is optional.

3. Deploy the updated configuration to API Gateway after changing any threat protection settings.

The recommended ModSecurity default configuration sets the engine mode to `SecRuleEngine DetectionOnly`, which applies the activated rules. This does not interfere with the traffic (does not block any requests).

You can also add threat protection profiles in **Environment Configuration > Libraries > Threat Protection Profiles > Add a Threat Protection Profile** in the Policy Studio tree.

#### Configure `modsecurity.conf` file

Depending on your environment, you might need to configure the settings in the `modsecurity.conf` file. For example:

* To handle an `application/xml` content type instead of `text/xml`, add the following line:

    ```
    SecRule REQUEST_HEADERS:Content-Type "application/xml" \
        "id:'200100',phase:1,t:none,t:lowercase,pass,nolog,ctl:requestBodyProcessor=XML"
    ```

* To configure ModSecurity to start denying requests with threatening content, change the value of `SecRuleEngine` from `DetectionOnly` to `On`.
* If you have not included the security action in your security rules, you might need to set `SecDefaultAction`. For more details, see [SecDefaultAction in the ModSecurity reference](https://github.com/SpiderLabs/ModSecurity/wiki/Reference-Manual-%28v2.x%29#SecDefaultAction).

For more details on the `modsecurity.conf` file format and recommended settings, see [Recommended Base Configuration](https://github.com/SpiderLabs/ModSecurity/wiki/Reference-Manual-%28v2.x%29#A_Recommended_Base_Configuration) in the ModSecurity documentation.

## Disable specific rules

A rule set configuration file contains a number of rules for a certain use case (for example, `REQUEST-942-APPLICATION-ATTACK-SQLI.conf`). However, some of the rules might create a false positive in your environment and need to be individually disabled.

To disable one or more rules it is not recommended to change the rule set configuration file itself, as this file is regularly updated and will be replaced. Instead, create your own rule file (for example, `activated_rules/disabledRule.conf`) and disable one or more rules using a directive:

```
SecRuleRemoveById 942101
```

For more information on this and other directives you can use, see [SecRuleRemoveById in the ModSecurity reference](https://github.com/SpiderLabs/ModSecurity/wiki/Reference-Manual-%28v2.x%29#SecRuleRemoveById).

## Apply DetectionOnly to specific rules

When `SecRuleEngine` is set to `On` all activated rules of the provided rule set are applied and block traffic. In some situations, for example, when a rule set is updated, you might not want to activate new or changed rules immediately as they might create false positives.

In this case, you can override the action of specific rules (to effectively apply `DetectionOnly` to these rules), and during a defined grace period monitor these new rules to ensure that they are not creating unwanted false positives.  

Use the following directive:

```
SecRuleUpdateActionById 942101 "pass,log"
```

For more information on this directive see [SecRuleUpdateActionById in the ModSecurity reference](https://github.com/SpiderLabs/ModSecurity/wiki/Reference-Manual-%28v2.x%29#SecRuleUpdateActionById).

## Use OWASP ModSecurity CRS version 3.x

To use CRS version 3.x, configuration files must be loaded by the web server in exactly this order:

1. `modsecurity.conf`
2. `crs-setup.conf`
3. `rules/*.conf`

To ensure that `crs-setup.conf` is loaded before the rules files, place the `crs-setup.conf` in the `activated_rules` directory and rename it to `0crs-setup.conf`.

You can verify that the configuration and rule files are loaded correctly in the API Gateway instance trace file at protocol level INFO:

```
Configure mod_security with /home/axway/Axway-7.7.20200130/apigateway/system/conf/threat-protection/default/modsecurity.conf
Processing 0crs-setup.conf
Processing REQUEST-901-INITIALIZATION.conf
Processing REQUEST-941-APPLICATION-ATTACK-XSS.conf
Processing ****.conf
```

Watch this video to see how to configure ModSecurity with CRS 3.x:

{{< youtube VrUBucXVGtk >}}

## Monitor API firewalling

Use the **Traffic > HTTP** tab in the API Gateway Manager web console to monitor API firewalling. You can use this tab to show how threat protection affects the HTTP traffic API Gateway serves.

![Monitor threat protection](/Images/APIGateway/admin_waf_monitor.png)

You can filter this tab to display by **Threat Protection** to quickly locate all passed or failed transactions.

1. Click **Filter +** in the search pane.
2. Select **Threat Protection** in the list.
3. Select a threat protection status in the dialog:

    **Pass**: The ModSecurity engine marks all transactions that pass its rules with this status.

    **Fail**: Transactions that violate any active ModSecurity engine rules are marked with this status. These transactions should be monitored because they represent a false positive the   protection rules might need to be adjusted), or malicious client traffic. You can view more details about the failure reason and specific rule violation by drilling own a specific   transaction and looking at the trace details.

4. Click **Apply**.

For example, the following shows detailed trace output from drilling down a failed transaction:

```
Message:Access denied with code 403 (phase 2).
Pattern match "(?i:(?:\\b(?:(?:s(?:ys\\.(?:user_(?:(?:t(?:ab(?:_column|le)|rigger)
|object|view)s|c(?:onstraints|atalog))|all_tables|tab)|elect\\b.{0,40}\\b
(?:substring|users?|ascii))|m(?:sys(?:(?:queri|ac)e|relationship|column|object)
s|ysql\\.(db|user))|c(?:onstraint ..." at ARGS:q. [file "C:\Axway-7.7\
apigateway\system\conf\threat-protection\default\activated_rules\
modsecurity_crs_41_sql_injection_attacks.conf"] [line "116"] [id "950007"]
[rev "2"] [msg "Blind SQL Injection Attack"] [data "Matched Data:SELECT *
FROM USERS found within ARGS:q:SELECT * FROM USERS"] [severity "CRITICAL"]
[ver "OWASP_CRS/2.2.9"] [maturity "9"] [accuracy "8"]
[tag "OWASP_CRS/WEB_ATTACK/SQL_INJECTION"] [tag "WASCTC/WASC-19"]
[tag "OWASP_TOP_10/A1"] [tag "OWASP_AppSensor/CIE1"] [tag "PCI/6.5.2"]
```

In addition to being written to trace files, ModSecurity report is also stored in the message attribute `modsec.error.message`. You can configure an alert policy that, for example, uses an **Alert** filter with a selector for this message attribute in the default message to pass the threat report to third-party monitoring systems.
