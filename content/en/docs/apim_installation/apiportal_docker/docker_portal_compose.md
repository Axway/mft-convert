{
"title": "Run API Portal using Docker Compose",
  "linkTitle": "Run using Docker Compose",
  "weight": "50",
  "date": "2021-01-05",
  "description": "Run API Portal Docker container with Docker Compose tool using the `docker-compose.yml` sample."
}
## Requirements

* [Docker Engine](https://docs.docker.com/engine/).
* [Docker Compose](https://docs.docker.com/compose/).
* API Portal Docker image, available from [Axway Support](https://support.axway.com/en/search/index/type/Downloads/q/API%20Portal%20/ipp/10/product/545/version/3036/subtype/89).
* API Portal Docker sample package, available from [Axway Support](https://support.axway.com/en/search/index/type/Downloads/q/API%20Portal%20/ipp/10/product/545/version/3036/subtype/88).

## About Docker Compose

Docker Compose is a simple way to run the whole API Portal solution with a single command. By using the `docker-compose.yml` file, provided with the API Portal Docker sample package, you get the following services preconfigured:

* MariaDB
* Redis
* API Portal

The `docker-compose.yml` file does not include API Manager and ClamAV configurations, you must configure them in API Portal separately. While API Manager is required for you to leverage your API Portal, ClamAV is an optional security tool. `docker-compose.yml` available in this package is for demo purpose only. Before you use it for production, you must modify all sensitive data at a minimum.

You can also modify the `sample.env` file, or replace it with your own in the `env_file` section, under `apiportal` service.

## Deploy with docker-compose.yml

To use `docker-compose.yml` to deploy your API Portal in containers:

1. Open `docker-compose.yml` file in an editor and locate the `image` section under `apiportal` service. Then, change the value to the name of your API Portal docker image.

    ```
    # ...
    services:
    # ...
      apiportal:
        image: <your-api-portal-image>
    ```

2. Run the following command to start the services, and wait API Portal service to get loaded:

    ```
    docker-compose up -d
    ```

    You can check the status of the load with the following command:

    ```
    docker-compose ps apiportal
    ```

When `Up (healthy)` is show under the **State** column, it means that API Portal is ready to handle requests.

If API Portal container does not become available within 30 seconds after you run the `up` command, check the logs for issues.

To check the logs, run:

```
docker-compose logs apiportal
```
