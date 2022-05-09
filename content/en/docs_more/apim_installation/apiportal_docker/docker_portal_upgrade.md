{
"title": "Upgrade API Portal container deployment",
  "linkTitle": "Upgrade container deployment",
  "weight": "30",
  "date": "2021-01-05",
  "description": "Upgrade your API Portal container deployment to a newer version."
}

To upgrade your API Portal container deployment, perform the following:

1. Obtain a newer API Portal Docker image, available from [Axway Support](https://support.axway.com/en/search/index/type/Downloads/q/API%20Portal%20/ipp/10/product/545/version/3036/subtype/89).
2. Remove the running container:

    ```
    docker container rm -f <old-container-name>
    ```

3. Recreate a new container with the same docker run parameters as the old one:

    ```
    docker container run <old-parameters> <newer-image>
    ```

The upgrade preserves any API Portal customizations stored in volumes or database.

{{% alert title="Note" %}}
There is no support for downgrading an API Portal container deployment. Running an older API Portal container against a newer database schema will result in failure.
{{% /alert %}}
