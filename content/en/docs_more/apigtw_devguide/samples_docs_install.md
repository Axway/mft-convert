{
"title": "Install and build code samples",
"linkTitle": "Install and build code samples",
"weight":"10",
"date": "2019-11-27",
"description": "Install and build API Gateway code samples."
}

## Install the code samples

Your installation of API Gateway includes code samples to demonstrate some of the tasks discussed in this guide, such as adding a custom filter or adding a message listener to API Gateway. The code samples are available in the `INSTALL_DIR/apigateway/samples/developer_guide` directory.

Alternatively, the associated code samples are available from [Axway Support](https://support.axway.com/) as a zip file.

This section describes how to install the code samples.

### Prerequisites

Before you install the code samples:

* You must install the API Gateway core server and Policy Studio, as the samples require certain classes that ship with these components to be on the CLASSPATH.
* To write custom message filters for API Gateway, you must install the samples on the same machine as API Gateway.

### Unzip the downloaded zip file

If you downloaded the samples from [Axway Support](https://support.axway.com/) as a zip file, the zip file contains the following directory structure:

```
developer-guide-7.7/samples/developer_guide
```

Use your preferred zip utility to unzip the file to a suitable location.

### Location of code samples

The location `DEVELOPER_SAMPLES` is used throughout this guide to refer to the location of the code samples:

* If you have installed API Gateway, `DEVELOPER_SAMPLES` refers to the `INSTALL_DIR/apigateway/samples/developer_guide` directory.
* If you have installed the code samples from a zip file, `DEVELOPER_SAMPLES` refers to the location where you installed the samples (for example, the `/home/samples/developer-guide-7.7/samples/developer_guide` directory).

## Build the code samples

API Gateway is built with JDK 1.8. To avoid `BadClassVersion` errors that might arise when deploying your sample classes with the API Gateway, you must also build the code samples with JDK 1.8.

This section describes how to build the code samples.

### Build the samples

Complete the following steps to build the samples:

1. Set the `VORDEL_HOME` environment variable to point to the root of your Axway API Gateway installation.

    For example, if you installed API Gateway in `/opt/Axway-7.7/apigateway`, set `VORDEL_HOME` to this directory.

2. Set the `POLICYSTUDIO_HOME` environment variable to point to the root of your Policy Studio installation.

    For example, if you installed Policy Studio in `/opt/Axway-7.7/policystudio`, set `POLICYSTUDIO_HOME` to this directory.

3. Set the `JAVA_HOME` environment variable to point to the root of a JDK 1.8 installation.

    For example, `/opt/jdk1.8.0_07`

4. Set the `JUNIT_HOME` environment variable to point to the directory containing your JUnit JAR file. The required version is 4.8.2.

    For example, `junit_4.8.2.jar`

5. Add Apache Ant to your `PATH` environment variable. For example, if Apache Ant is installed in `/opt/ant`, add `/opt/ant/bin` to your `PATH`. See the [Apache Ant website](http://ant.apache.org/) for more information on Apache Ant.
6. Change to the directory where the sample is installed. Each sample is installed under `DEVELOPER_SAMPLES/SAMPLE_NAME` (for example, `DEVELOPER_SAMPLES/FilterInterceptorLoadableModule`).
7. Open the `README.TXT` file and follow the instructions to build and run the sample.

### Description of samples

The following code samples are included:

* `DEVELOPER_SAMPLES/FilterInterceptorLoadableModule` – Sample classes that implement Java interfaces. For more information, see [Java interfaces for extending API Gateway](/docs/apigtw_devguide/java_extend_gateway).
