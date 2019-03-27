# Section 1: Rubrik API Basics

The Rubrik REST API provides a RESTful interface for working with Rubrik clusters and Rubrik Edge virtual appliances. The Rubrik REST API can be used to query, configure, and control nearly all of the operations of the Rubrik software.

Through authenticated and encrypted interaction with the Rubrik REST API server, perform any of the operations that are available through the Rubrik web UI and many bulk-type operations that might otherwise be difficult or impossible to perform.

This documentation provides reference information and examples of typical workflows for the Rubrik REST API. For more detailed information about working with Rubrik clusters and Rubrik Edge virtual appliances refer to the Rubrik User Guide.

A quick way to become familiar with the Rubrik REST API, is to use the Rubrik REST API Explorer. 

## Exercise 1-1: Rubrik CDM API Documentation
In Google Chrome, open a new tab and type `https://<node-ip-address>/docs/v1`. The node IP information can be found in [Lab Topology](/lab-topology.md).

The Rubrik RESTful API (v1) web page opens.

![API Docs](/img/image1.png)

Browse through the Rubrik CDM API documentation and note the different endpoints available to you. 

| Note: Rubrik CDM API documentation is available online as well. You can find it [here](https://github.com/rubrikinc/api-documentation) |
| --- |

## Exercise 1-2: Rubrik CDM API Explorer 
In Google Chrome, open a new tab and type `https://<node-ip-address>/docs/v1/playground`. The node IP information can be found in [Lab Topology](/lab-topology.md).

The Rubrik RESTful API Explorer page opens.

![API Explorer-v1](/img/image2.png)

On the top-right hand of the page, click **Authorize**. The Available Authorizations page opens.

![Available Authorizations](/img/image3.png)

Enter the Rubrik CDM credential information found in [Lab Topology](/lab-topology.md).

Click **Authorize**.

In Google Chrome, open a new tab and type `https://<node-ip-address>/docs/internal/playground` The Rubrik RESTful API Explorer page opens.

![API Explorer-internal](/img/image4.png)

## Exercise 1-3: Rubrik CDM API Authentication (using curl) and using the API from command line
In Google Chrome, open a new tab and type `https://<node-ip-address>/docs/v1/#section/Authentication/Authentication-session`. Review the chapter about authentication. 

Generate a bearer token using the BasicAuth method using the curl command:

`curl -k -u admin:pass -X POST "https://$cluster_address/api/v1/session"`

The Rubrik REST API server returns to following response body:

```
{
  “id”: “$session_id”,
  “organization_id” : “$organization_id”,
  “userID” : “$user_id”,
  “token”: “$token_id”,
  “expiration”: “$expiration”
}
```
 
 Copy your token (`$token_id`) to get the Rubrik cluster software version, and the API version. In this curl command, the GET request to `/cluster` uses the `-H` flag to provide the Authorization: Bearer `$token_id` value.

```
curl -k -H "Authorization: Bearer $token_id" -X GET
  "https://$cluster_address/api/v1/cluster/me"
```

The response body contains an array with the session ID, the Rubrik cluster software version, and the API version.

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

Notice the `-k` flag, why is the `-k` flag required?

| ANSWER: `-k` to bypass an alert about the self-signed certificate. See `https://<node-ip-addres>/docs/v1/#section/Authentication` for more information |
| --- |

Try the same command without the `-k` flag

# Section 2: Getting Around Using an API


## Exercise 2-1: Using the API Explorer to generate a curl command to get cluster info 
In Google Chrome, open a new tab and type `https://<node-ip-address>/docs/v1/playground`
 
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

# Section 3: Retrieving Information for Daily Operations/Tasks

## Exercise 3-1: Using the API Explorer to get Number of Unassigned VMs (VMware)

Instead of the UI dashboard you can also find the Unprotected VMs using the API. This exercise will teach you learn how to use parameters.

Use the API explorer to find all the VMware VMs without an SLA assigned. 

![Unassigned VMs](/img/image6.png)

Note how parameters are used in the URL. 

Review the response body to find out how many VMs have no SLA assigned.

![Unassigned VMs Response](/img/image7.png)

Use the API Explorer to find the first VMware VM without an SLA assigned. Hint: use a parameter to specify the amount of VMs you want to report on.

## Exercise 3-2: Using the API Explorer to Get Information on a Specific Object

The Rubrik CDM API uses IDs to refer to objects; for instance, a specific VMware VM. You can use the API Explorer to find the object ID for a specific VM. 

![Object ID](/img/image8.png)

You can also use the GUI to find a specific VM by name, and then copy the ID from the URL.

![Object ID-GUI](/img/image9.png)

Find the object ID for a specific VM and then paste the ID into the right parameter to get the details for that VM. 

![Paste ID](/img/image10.png)

The ID is often used across endpoints; the following image provides another example where the ID is used to get a list of snapshots for a specific VM.

![Use ID for Snapshot List](/img/image11.png)

## Exercise 3-3: find if there are missed snapshots for a specific VM 

Using the ID gathered in the previous exercises, find the endpoint and check if there were any missed snapshots for that specific VM. 

Endpoint to find missing snapshots:

`https:// <node-ip-addres>/docs/v1/playground/#!/47vmware47vm/missedSnapshots`

## Exercise 3-4: Get cluster run rate

One important metric for daily operations is the cluster run rate, in other words: show how long do I have before the cluster runs out of storage capacity.

`https://<node-ip-address>/api/internal/stats/runway_remaining`
 
![Runway Remaining](/img/image12.png)

## Exercise 3-5: Get status info and counters about backup jobs for the past 24 hours.

`https:// <node-ip-address>/docs/internal/playground/#!/47event/queryEventJobCountByStatus`

![By Count](/img/image13.png)

 

# SECTION 6 Other useful tricks and tools working with REST api’s

## Exercise 6-1: How to create a csv output from JSON data.

List all vm using the api explorer and copy all the data from the response body.

Go to the following website https://json-csv.com/
Paste your json in the paste box.

This will generate a CSV file for you or even more
Explore the other formats .

Note: this is site is not related to Rubrik, it’s your own responsibilty to decided wether you can use this site or not with your data.

# Exercise: use postman with Rubrik’s rest api.

Download postman and install it on your computer, there is also a postman chrome plugin.

Note: postman is an external tool without any relation to Rubrik.

Play with postman to see how it works 
Below is one example how to use postman for a GET request to get you started
Important remarks: you have to select basic auth the fill in you password and username.



Try to do a couple of the exercises using postman also use the post method.

Postman has a very powerfull feature, it’s can generate code for the api calls.


See example below.



















## Exercise 6-2: How to use google chrome to explore how the Rubrik GUI uses the rest api.

Enable developer tools in chrome while you are in the Rubrik GUI.

<need an english browser example here>

Your browser while look like this :



On the right hand pane you will see which api’s are used by the Rubrik gui.

In this exercise we are going to create a on demand snapshot of the VM you have snap shotted in the previous exercise. With same SLA domain you have used in the previous exercise.

Make sure you select the network tab in the developer pane. And clear the screen just before you start the snapshot.





After the you created snapshot your screen should look like this:



Now click on the snapshot api call (annotated in the example)


With developer tools you can inspect headers and which end points are called with the method. So you can learn which end points you need to perfrom certain actions.

If you scroll down in the righthand pane you can also see the request payload if the request method is POST:


This is the SLA id you have also specified in the previous exercise.