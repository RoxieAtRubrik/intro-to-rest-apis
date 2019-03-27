# Section 1: Rubrik API Basics

The Rubrik REST API provides a RESTful interface for working with Rubrik clusters and Rubrik Edge virtual appliances. The Rubrik REST API can be used to query, configure, and control nearly all of the operations of the Rubrik software.

Through authenticated and encrypted interaction with the Rubrik REST API server, perform any of the operations that are available through the Rubrik web UI and many bulk-type operations that might otherwise be difficult or impossible to perform.

This documentation provides reference information and examples of typical workflows for the Rubrik REST API. For more detailed information about working with Rubrik clusters and Rubrik Edge virtual appliances refer to the Rubrik User Guide.

A quick way to become familiar with the Rubrik REST API, is to use the Rubrik REST API Explorer. 

## Exercise 1-1: Rubrik CDM API Documentation
In Google Chrome, open a new tab and type `https://<node-ip-address>/docs/v1`. The node IP information can be found in [Lab Topology](/lab-topology.md).

The Rubrik RESTful API (v1) web page opens (shown below).

![API Docs](/img/image1.png)

Browse through the Rubrik CDM API documentation and note the different endpoints available to you. 

| Note: Rubrik CDM API documentation is available online as well. You can find it [here]()

## Exercise 1-2: Rubrik CDM API Explorer 
In Google Chrome, open a new tab and type `https://<node-ip-address>/docs/v1/playground`. The node IP information can be found in [Lab Topology](/lab-topology.md).

The Rubrik RESTful API Explorer page opens (shown below).



On the top-right hand of the page, click Authorize. The Available Authorizations page opens (shown below).

Authorize using the following:
username - admin
password - Welcome10!Rubrik
Click Authorize.
In Google Chrome, open a new tab and type https://<node-ip-address>/docs/internal/playground The Rubrik RESTful API Explorer page opens (shown below).




Exercise 2-3: Rubrik RESTful API Authentication (using curl) and using the API from the command line
In Google Chrome, open a new tab and type https://<node-ip-address>/docs/v1/#section/Authentication/Authentication-session
Read the chapter about the authentication. 
Generate a bearer token using the BasicAuth method using the curl command:
curl -k -u admin:pass -X POST "https://$cluster_address/api/v1/session"
The rubrik REST API server returns to following response body:
{
  “id”: “$session_id”,
  “organization_id” : “$organization_id”,
  “userID” : “$user_id”,
  “token”: “$token_id”,
  “expiration”: “$expiration”
}
 Copy your token ($token_id) to get the Rubrik cluster software version, and the API version. In this curl command, the GET request to /cluster uses the -H flag to provide the Authorization: Bearer $token_id value.

curl -k -H "Authorization: Bearer $token_id" -X GET
  "https://$cluster_address/api/v1/cluster/me"
The response body contains an array with the session ID, the Rubrik cluster software version, and the API version.
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
Notice the -k flag, why is the -k flag required? (see answer in the appendix)
Try the same command without the -k flag


SECTION 3 Getting around with API
Exercise 3-1: Using the API Explorer to generate a curl command to get cluster info 

In Google Chrome, open a new tab and type https://<node-ip-address>/docs/v1/playground
 Select the /api/v1/cluster end point


Press the ‘Try it out!’ button
Copy and paste the curl command
Execute the curl command in the CLI
Why does it give a error?
Fix the error by specifying the right flag. The correct output should look this:
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

Section 4: Retrieving Information for Daily Operations/Tasks
Exercise 4-1: Using the API Explorer to get the number of Unassigned VMs (VMware)

Instead of the UI dashboard you can also find the Unprotected VMs using the API, in this exercise you learn how to use parameters.

Use the API explorer to find all the VMware VMs without an SLA assigned    . 




Please take note of how parameters are used in the URL
Browse through the response body to find out how many VMs have no SLA assigned



Use the API explorer to find the first VMware VM without an SLA assigned. Hint: specify the amount of VMs you want to report on with a parameter




Exercise 4-2: Using the API Explorer to get the number of unassigned VMs (Hyper-V)

Figure out the curl command for getting a list of Hyper-V VMs with no SLA assigned. Hint: info about Hyper-V VMs can be found through the internal REST API
See answer in the appendix

Exercise 4-3: Using the API Explorer to get info for a specific object

The Rubrik API uses IDs to refer to objects, for instance a specific VMware VM. You can use the API Explorer to find the object ID for a specific VM. See screen shot below:




You can also use the GUI to find a specific VM by name, and then copy the ID from the URL, see example below:




Find the object ID for a specific VM 
Paste the ID you have found into the right parameter to get the details for that specific VM



The id is used in a lot of endpoints, here is another example where the id is used to get a list of snapshots for a specific vm.




Exercise 4-4: find if there are missed snapshots for a specific VM 

Using the ID you found in the previous exercises.
Find the endpoint and check if there were any missed snapshots for that specific VM. 

See appendix for the endpoint.


Exercise 4-5: Get cluster run rate

One important metric for daily operations is the cluster run rate, in other words: show how long do I have before the cluster runs out of storage capacity.

https://<node-ip-address>/api/internal/stats/runway_remaining


 

Exercise 4-6: Get status info and counters about backup jobs for the past 24 hours.

https:// <node-ip-address>/docs/internal/playground/#!/47event/queryEventJobCountByStatus



 



Section 5: Advanced API commands

So far we only executed API calls which query data. (GET -method) . 
There are also api calls which use different methods like POST, PATCH and DELETE with these methods you can actually change data or start actions on a Rubrik cluster. 

Exercise 5-1: Identify the different methods in the API Explorer




Exercise 5-2 : Live mount a VM with the api explorer

Get the latest snapshot for a vm

Copy snapshot id
Paste id in livemount endpoint (notice this API end uses the POST method)
Execute live mount



Check the status of the livemount in the GUI


(optional exercise) check the status of the vm in vsphere)

After the VM is mounted 

Unmount the vm in the gui.


Exercise 5-3: Live mount the same snapshot again. And change the name of VM

With the get method which we used in the previous exercises you can control the api-call using parameters. 

With the post method you have to ‘post’ the data in what is called the payload which will be includied in the body of the api call. The payload is formated in JSON. 

In the example you can see how easy it is the generate the payload in the right (JSON) format.





After mounting the VM check if the VM is mounted (in the GUI). Notice the name change of the mounted vm.
This is an example that you have more option with the API than you have in GUI. It’s not possible to livemount a VM with another name using the GUI.
Unmount the vm in the gui. 
Copy and paste the curl command in the gui to see how the payload looks like in the GUI.

curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Basic <$token_id>' -d '{ \ 
   "vmName": "rubrik_api_training" \ 
  \ 
 }' 'https://<node-ip-address>/api/v1/vmware/vm/snapshot/ac723e1c-80a5-4dad-bbc3-eb7941017145/mount'

As you can see in above example , the curl command to excute the livemount can become pretty long and complex.



Exercise 5-4 : Create an on-demand snapshot for a vm.

Get an object id for a vm for which you want snapshot on demand (see previous exercise for an example)

Create a on-demand snapshot for this VM using a different sla than the one which is assigned to that VM.  Hint: You can’t specify the name for SLA in the payload.



Check in the gui if your on-demand snapshot has been started and executed. 

SECTION 6 Other useful tricks and tools working with REST api’s

Exercise 6-1: How to create a csv output from JSON data.

List all vm using the api explorer and copy all the data from the response body.



Go to the following website https://json-csv.com/
Paste your json in the paste box.





This will generate a CSV file for you or even more
Explore the other formats .

Note: this is site is not related to Rubrik, it’s your own responsibilty to decided wether you can use this site or not with your data.

Exercise: use postman with Rubrik’s rest api.

Download postman and install it on your computer, there is also a postman chrome plugin.

Note: postman is an external tool without any relation to Rubrik.

Play with postman to see how it works 
Below is one example how to use postman for a GET request to get you started
Important remarks: you have to select basic auth the fill in you password and username.



Try to do a couple of the exercises using postman also use the post method.

Postman has a very powerfull feature, it’s can generate code for the api calls.


See example below.



















Exercise 6-2: How to use google chrome to explore how the Rubrik GUI uses the rest api.

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


Appendix:

Exercise 2-3
why is the -k flag required in a curl command?
-k to bypass an alert about the self-signed certificate
See also https://<node-ip-addres>/docs/v1/#section/Authentication
Exercise 4-2
Curl command to find unassigned HyperV VM’s: 
curl -X GET --header 'Accept: application/json' --header 'Authorization: Basic $token_id' 'https://<node-ip-adres>/api/internal/hyperv/vm?sla_assignment=Unassigned'
request url: https://sand1-rbk01.rubrikdemo.com/api/internal/hyperv/vm?sla_assignment=Unassigned
Exercise 4-4
Endpoint to find missing snapshots:
https:// <node-ip-addres>/docs/v1/playground/#!/47vmware47vm/missedSnapshots