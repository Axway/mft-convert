{
    "title": "About system users",
    "linkTitle": "About system users",
    "weight": "190"
}An APF, authorized program facility, is a security element that allows an installation to identify system or user programs. When a Transfer CFT system does not use an APF, the Transfer CFT USERCTRL parameter has no effect on file permissions and all file actions are done by the account that started Transfer CFT. This means that to enable user control for file permissions, your Transfer CFT requires APF.

### Non-APF installations

In non-APF installations, Transfer CFT is started by the user USERMON and the USERCTRL does not affect file rights or actions.

### APF installations with USERCTRL

#### Receiver side

While Transfer CFT is still started by the user USERMON, in an APF installation the USERCTRL setting has a direct effect on the transfer procedure.

1. USERCTRL=NO USERID=TEST
    -   A receive is performed by the user USERMON
    -   TEST submits the end-of-transfer procedure
1. USERCTRL=NO USERID=
    &lt;ul&gt;&lt;li&gt;A receive is performed by the user USERMON&lt;/li&gt;&lt;li&gt;USERMON submits the end-of-transfer procedure&lt;/li&gt;&lt;/ul&gt;&lt;/li&gt;
1. USERCTRL=NO USERID=NON RACF USERID (not in RACF database)
    &lt;ul&gt;&lt;li&gt;A receive is performed by the user USERMON&lt;/li&gt;&lt;li&gt; USERMON submits the end-of-transfer procedure&lt;/li&gt;&lt;/ul&gt;&lt;/li&gt;
1. USERCTRL=YES USERID=TEST
    &lt;ul&gt;&lt;li&gt;A receive is performed by the user TEST&lt;/li&gt;&lt;li&gt; TEST submits the end-of-transfer procedure&lt;/li&gt;&lt;/ul&gt;&lt;/li&gt;
1. USERCTRL=YES USERID=
    &lt;ul&gt;&lt;li&gt;A receive is performed by the user USERMON&lt;/li&gt;&lt;li&gt;USERMON submits the end-of-transfer procedure&lt;/li&gt;&lt;/ul&gt;&lt;/li&gt;
1. USERCTRL=YES USERID=NON RACF USERID (not in RACF database)
    -   Receive is not performed
    -   End-of transfer procedure not submitted

Release a resource for receive transfers
----------------------------------------

You do not usually need to switch users to perform a resource release. To activate a switch during FREE, set the ..UPARM(CNFENV) FREE_AS_USER variable to 1.

#### Sender side

1. USERCTRL=NO USERID=TEST
    &lt;ul&gt;&lt;li&gt;A send is performed by the user USERMON&lt;/li&gt;&lt;li&gt;TEST submits the end-of-transfer procedure&lt;/li&gt;&lt;/ul&gt; &lt;![CDATA[ ]]&gt;&lt;/li&gt;
1. USERCTRL=YES USERID=TEST
    &lt;ul&gt;&lt;li&gt;A send is performed by the user TEST&lt;/li&gt;&lt;li&gt;The transfer procedure is submitted by TEST&lt;/li&gt;&lt;/ul&gt;&lt;/li&gt;

> **Note**
>
> Note: Setting the UCONF cft.server.exec_as_user variable to ‘NO’ also directly effects the transfer procedure.
