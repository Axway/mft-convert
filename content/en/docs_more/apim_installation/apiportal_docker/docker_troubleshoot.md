{
  "title":"Troubleshooting API Portal in Docker",
  "linkTitle":"Troubleshooting",
  "weight":"60",
  "date":"2019-08-09",
  "description":"Troubleshoot problems you might encounter when running API Portal in Docker containers."
}

This section describes docker related issues only. For information on general troubleshooting, see [Troubleshooting API Portal](/docs/apim_administration/apiportal_admin/troubleshooting/).

## View logs

API Portal container redirects Apache and API Portal logs to stdout and stderr, so you can run the following command to view the logs:

```
docker container logs -f <container-name>
```

Alternatively you can run API Portal in non-detached mode, that is, without `-d` flag. This will let you see logs in real time from the very beginning of the container boot. This is not recommended for production installations, only for testing.

In most cases, the logs help to detect the cause of the problem.

## Apache does not start

After the API Portal container is started, the Apache service is not started and you cannot access your API Portal.

To check this issue, ensure that you have waited sufficient time for the container to start. When running the container for the first time, depending on your database load and connection speed, it can take a while to import the whole database schema and initial data. To check if this is the case, read the Docker logs for the API Portal container.

## API Portal UI does not load

When you try to access API Portal from a browser you get the following message: `Error displaying the error page.`

Perform the following checks to try and identify the cause of the problem:

* Check whether the database is running and the API Portal schema is available.
* Connect to the API Portal container, and check whether it can access the database schema from the database container.
* Connect to the API Portal container, open `/opt/axway/apiportal/htdoc/configuration.php` and verify that the database settings are correct (host, port, user, password, db, and so on).
* If you cannot identify the cause of the problem, contact [Axway Support](https://support.axway.com).
