---

    title: Swagger UI usage
    linkTitle: Swagger UI usage
    weight: 320

---
The Swagger UI is both documentation and a client for the REST API. Each supported API command provides a detailed description, including possible parameters.

Swagger builds the HTTP frames and sends them to the REST server. However, before sending a command you must provide credentials.

#### Authorization

****Basic authorization****

Enter your {{< TransferCFT/axwayvariablesComponentLongName  >}} login name and password in the **Username/Password** fields. Click **Authorize**.

****Bearer authorization****

Copy your token from the {{< TransferCFT/axwayvariablesComponentLongName  >}} UI <span class="bold_in_para">****My Access Tokens****</span> page into the <span class="bold_in_para">****Value**** </span>field and click Authorize. Click **Authorize**.

![](/Images/TransferCFT/authorization_swagger.png)

> **Note**
>
> See Generate an access token.

#### Try it out option

The <span class="bold_in_para" style="font-style: italic;">******Try it out******</span> option allows you to execute API requests from within the Swagger UI.
