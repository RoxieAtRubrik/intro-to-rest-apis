# Lesson 3: Retrieving Information for Daily Operations/Tasks

## Lesson 3-1: Using the API Explorer to get Number of Unassigned VMs (VMware)

Instead of the UI dashboard you can also find the Unprotected VMs using the API. This exercise will teach you learn how to use parameters.

Use the API explorer to find all the VMware VMs without an SLA assigned. 

![Unassigned VMs](/img/image6.png)

Note how parameters are used in the URL. 

Review the response body to find out how many VMs have no SLA assigned.

![Unassigned VMs Response](/img/image7.png)

Use the API Explorer to find the first VMware VM without an SLA assigned. Hint: use a parameter to specify the amount of VMs you want to report on.

## Lesson 3-2: Using the API Explorer to Get Information on a Specific Object

The Rubrik CDM API uses IDs to refer to objects; for instance, a specific VMware VM. You can use the API Explorer to find the object ID for a specific VM. 

![Object ID](/img/image8.png)

You can also use the GUI to find a specific VM by name, and then copy the ID from the URL.

![Object ID-GUI](/img/image9.png)

Find the object ID for a specific VM and then paste the ID into the right parameter to get the details for that VM. 

![Paste ID](/img/image10.png)

The ID is often used across endpoints; the following image provides another example where the ID is used to get a list of snapshots for a specific VM.

![Use ID for Snapshot List](/img/image11.png)

## Lesson 3-3: find if there are missed snapshots for a specific VM 

Using the ID gathered in the previous exercises, find the endpoint and check if there were any missed snapshots for that specific VM. 

Endpoint to find missing snapshots:

`https:// <node-ip-addres>/docs/v1/playground/#!/47vmware47vm/missedSnapshots`

## Lesson 3-4: Get cluster run rate

One important metric for daily operations is the cluster run rate, in other words: show how long do I have before the cluster runs out of storage capacity.

`https://$cluster_address/api/internal/stats/runway_remaining`
 
![Runway Remaining](/img/image12.png)

## Lesson 3-5: Get status info and counters about backup jobs for the past 24 hours.

`https:// $cluster_address/docs/internal/playground/#!/47event/queryEventJobCountByStatus`

![By Count](/img/image13.png)