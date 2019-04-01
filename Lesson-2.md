# Lesson 2: Using a REST API to Retrieve Information



## Lesson 2-1: Using the API Explorer to List SLA Domains

The API Explorer is a [Swagger](https://swagger.io/) page that documents the REST API published by this server. We’ll use it to demonstrate how REST APIs are consumed by applications.

To use Rubrik’s API playground:

In Google Chrome, open a new tab and type `https://$cluster_address/docs/v1/playground`. The node IP information can be found in [Lab Topology](/lab-topology.md).

In the top right-hand corner, click **Authorize**.



Ensure your assigned domain username and password. Click **Authorize**.

Navigate to `/sla_domain` and click **Show/Hide**. Click **Get** list of SLA Domains.



Under Parameters, enter a Value for name.
Click **Try it out!**



Notice the Response Body. There should be at least one response for Gold. This details the configuration of queried SLA Domain. 



## Lesson 2-2: Generate a cURL Command to Get Cluster Info

Select the `/api/v1/cluster` endpoint

![Cluster Endpoint](/img/image5.png)

Press the **Try it out!** button

Copy and paste the curl command

Execute the curl command in the CLI

Why does it give an error?

Fix the error by specifying the right flag. The correct output should look similar to this:

```
{
  "id":"cc19573c-db6c-418a-9d48-067a256543ba",
 "version":"5.0.0~DA1-583",
  "apiVersion":"1",
  "name":"sand1rbk01",
  "timezone":{"timezone":"America/Los_Angeles"},
 "geolocation":{"address":"Santa Clara, CA, USA"},
 "acceptedEulaVersion":"1.0", 
 "latestEulaVersion":"1.0"
}
```