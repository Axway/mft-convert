{
    "title": "Connect to Axway Mobile Backend Services",
    "linkTitle": "Connect to Axway Mobile Backend Services",
    "weight": "150",
    "date": "2019-09-17",
    "description": "Connect to Axway Mobile Backend Services from API Manager."
}

This topic explains how to connect to Axway Mobile Backend Services from API Manager to virtualize Mobile Backend Services APIs in your API Catalog and add an extra layer of security for your apps. For example, this includes adding quota, threat protection, and user authentication and authorization policies.

{{< alert title="Note" color="primary" >}}Axway Mobile Backend Services were previously known as Appcelerator ArrowDB and ArrowPush services, and these names remain in some internal components and documentation.{{< /alert >}}

## Mobile Backend Services use cases

Axway Mobile Backend Services provide pre-built, scalable, cloud, and mobile-specific back-end services using REST APIs and data objects. For example, these include location-based services, social media integration, geo-location, photos, media handling, and so on.

Mobile Backend Services also include the ability to send push notifications to Android and iOS apps, and Software Development Kits (SDKs) for integration with the following mobile apps:

* Android
* iOS
* Node.js

Mobile app developers can call the Mobile Backend Services APIs to integrate with their apps. For example, this enables you to add important mobile features without server coding or administration. You can focus on client-side development, reducing overall app time to market. Mobile Backend Services also enable server-side hosting, scalability, and administration for Appcelerator public and VPC options. There is no need for installation, you can call Mobile Backend Services APIs and existing objects, and use features that apps need instead of providing generic data storage.

## Mobile Backend Services REST API

The Mobile Backend Services REST API is accessible from any networked client device and enables creating, querying, updating, and deleting Mobile Backend Services objects. Each Mobile Backend Services object has its own URL and HTTP methods (`GET`, `POST`, `PUT`, or `DELETE`).

To make an API call, you make an HTTP request, and API responses are returned as JSON objects. For example:

```
https://api.cloud.appcelerator.com/v1/checkins/show.json?key=<YOUR_APP_KEY>&checkin_id=4d8bc645d0afbe0363000013
```

User sessions and cookies must be saved and reused with each API call (if login is required). For example, you can pass the session ID to the `_session_id` parameter in the URL:

```
https://api.cloud.appcelerator.com/v1/reviews/create.json?key=<API_KEY>&_session_id=<SESSION_ID>
```

For more details, see the [Mobile Backend Services documentation](https://docs.axway.com/bundle/Mobile_Backend_Services_allOS_en/).

## Create a Mobile Backend Services app

Before you can virtualize the Mobile Backend Services API in API Manager, you must first create a Mobile Backend Services app, and an app key and user for the Mobile Backend Services API.

Perform the following steps:

1. Connect to the Mobile Backend Services dashboard in your browser, and enter your credentials. For example:

    ```
    <https://platform.appcelerator.com/>
    ```

2. In the dashboard, click the **+** sign in the toolbar at the top, and select **Create ArrowDB Datasource** from the drop-down list.
    ![Create Mobile Backend Service app](/Images/docbook/images/api_mgmt/mbs_create_datasource.png)
3. Enter a meaningful **Name** for your app, and click **OK**.
4. When your new app is selected, click the **Configuration** tab on the left.
    ![Show Mobile Dataservices App Key](/Images/docbook/images/api_mgmt/mbs_show_app_key.png)
5. On the **Keys** tab, under **App Key**, click **Show**.
6. Copy the app key and save for later use (for example, paste to a file).
7. Click **Manage Data** to display all the managed data objects for the app.
8. Click the **Users** link at the bottom of the **Managed Data Objects** table:
    ![Create Mobile Backend Services User](/Images/docbook/images/api_mgmt/mbs_create_user.png)
9. Click **Create User** on the top right, and enter the user details. For example, the **Username**, **Email**, and **Password** fields are required. You should also select **Yes** in the **Admin** field.
10. When finished, click **Save Changes** at the bottom left.

## Virtualize the Mobile Backend Services API in API Manager

When you have created a Mobile Backend Services app, app key, and user, you can then virtualize the Mobile Backend Services API in API Manager.

Perform the following steps:

1. Get the Swagger URL for Mobile Backend Services APIs. For example:

    ```
    <https://admin.cloudapp-enterprise.appcelerator.com/arrowdbapi/swagger.json>
    ```

2. To create a new back-end API from Swagger in API Manager, select **API > Backend API > New API > Import Swagger API**.
3. In the **Import API** dialog, complete the following:
    * **Source**: Select **Swagger definition URL** from the list.
    * **URL**: Paste the Swagger URL (in this case, `<https://admin.cloudapp-enterprise.appcelerator.com/arrowdbapi/swagger.json>`).
    * **API Name**: Enter a user-friendly name for the API (for example, `MyTest MBS API`).
    * **Organization**: Select the organization from the list (for example, **Acme Inc**).
    * **Authentication**: Enter the **User name** and **Password** for the API if required.
4. To create the front-end in API Manager, select **API** > **Frontend API** > **New API** > **New API from backend API** and select the existing API from the list (in this case, `MyTest MBS API`).
5. In the front-end API, click the **Inbound** tab, and in the **Outbound security** field, select **API Key**, and click **OK**.
6. Click the **Outbound** tab, and in the **Outbound authentication profile** field, select **API Key**.
7. In the **API Key Authentication** dialog, paste in the **API key** for your Mobile Backend Services app that you created in [Create a Mobile Backend Services app](#create-a-mobile-backend-services-app), and click **OK**.
8. Click **Save**.

The following example in API Manager shows the API key created earlier when creating the Mobile Backend Services app:

![Configure API Key Authentication](/Images/docbook/images/api_mgmt/mbs_frontend_app_key.png)

### Verify the Mobile Backend Services API virtualization in API Manager

You can test the virtualization of the Mobile Backend Services API using the **Try it** feature in the API Manager API Catalog. Alternatively, you can use third-party test tools such as curl or Postman.

Most of the Mobile Backend Services API methods require user authentication. Therefore you should call the login API before calling other APIs. The following example shows using curl to test user login with the virtualized URL:

```
curl -X POST https://<apimanagerhost>:8065/db/v1/users/login.json -H 'content-type: application/x-www-form-urlencoded' -d 'login=<username>&password=<password>'
```

You can specify the Mobile Backend Services username and password that you created earlier.

## Generate an SDK for virtualized Mobile Backend Services APIs in API Portal

{{< alert title="Note" color="primary" >}}Use the API Portal SDK generator rather than Mobile Backend Services SDK. {{< /alert >}}

You can generate an SDK for virtualized Mobile Backend Services APIs in API Portal. SDK generation must first be enabled in API Portal, see [Customize API Catalog](/docs/apim_administration/apiportal_admin/customize_apicatalog_overview/).
