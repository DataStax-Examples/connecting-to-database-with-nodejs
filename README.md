# Switching Connections between DDAC/C*/DSE or Apollo with NodeJs
This application shows how to use configure your NodeJs application to connect to Cassandra or an Apollo database at runtime.

Contributors: [Dave Bechberger](https://github.com/bechbd) based on [this](https://github.com/datastax/nodejs-driver/blob/master/examples/basic/basic-connect.js) example

## Objectives
* To demonstrate how to specify at runtime between a Cassandra client configuration and an Apollo configuration for the same application.

## Project Layout
* [app.js](app.js) - The main application file which contains all the logic to switch between the configurations

## How this Sample Works
This sample uses environment variables to specify the configuration parameters and whether to use a Cassandra configuration or an Apollo configuration.  All the logic to switch between the configurations occurs in the `getClientConfiguration` function.  
* If you specify the `USEAPOLLO` environment variable:
   	* The environment variables are checked to see that `DBUSERNAME`, `DBPASSWORD`, `SECURECONNECTBUNDLEPATH`, and `KEYSPACE` exist
		* If they exist then the Apollo connection is created
		* If they do not exist then an error is thrown
* If you so not specify the `USEAPOLLO` environment variable:
   	* The environment variables are checked to see that `CONTACTPOINTS` and `DATACENTER` exist
		* If they exist then the standard connection is created
		* If they do not exist then an error is thrown
		* If `DBUSERNAME` and `DBPASSWORD` exist then credentials are added to the configuration
		* If `KEYSPACE` exists then it is added to the configuration

While these criteria added to the configuration are commonly used configuration parameters you are able to specify any additional ones in the code.

## Setup and Running

### Prerequisites
* Node 4 and above
* A Cassandra cluster or an Apollo database to connect to with the appropriate connection information

Additionally while this example uses environment variables to control which configuration is selected this could also be done via the use of configuration files or command line parameters.

For clarity this sample does not contain any of the normal error handling process you would want to wrap around connecting to a cluster to handle likely errors that would occur.

### Running

To connect to an Apollo database you first need to download the secure connect bundle following the instructions found [here](https://docs.datastax.com/en/landing_page/doc/landing_page/cloud.html).

Once you have this information you can run this to connect to Apollo by using the command below, with the appropriate configuration added:

`USEAPOLLO=true DBUSERNAME=XXX DBPASSWORD=XXX KEYSPACE=XXX SECURECONNECTBUNDLEPATH="/valid/path/to/secureconnectbundle.zip" node app.js`

If you would like to connect to a Cassandra cluster use the command below, with the appropriate configuration added:

`CONTACTPOINTS=XX.XX.XX.XX DATACENTER=XXXX node app.js`

Once you run this against either option you will get output that specifies the number of hosts and their IP addresses printed to the console:

```Connected to cluster with 3 host(s) ["XX.XX.XX.136:9042","XX.XX.XX.137:9042","XX.XX.XX.138:9042"]```
