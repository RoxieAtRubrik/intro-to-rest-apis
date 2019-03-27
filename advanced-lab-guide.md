| **Under Construction** | 
| ---|

# Section 4: Advanced API commands

So far we only executed API calls which query data. (GET -method) . 
There are also api calls which use different methods like POST, PATCH and DELETE with these methods you can actually change data or start actions on a Rubrik cluster. 

## Exercise 4-1: Identify the different methods in the API Explorer

![Methods](/img/image101.png)


## Exercise 4-2 : Live mount a VM with the API explorer

Get the latest snapshot for a vm

![GET Snapshot ID](/img/image102.png)

Copy snapshot id
Paste id in livemount endpoint (notice this API end uses the POST method)
Execute live mount

![Mount VM](/img/image103.png)

Check the status of the livemount in the GUI

![Live Mount Status](/img/image104.png)

(optional exercise) check the status of the vm in vsphere)

After the VM is mounted 

Unmount the vm in the gui.


## Exercise 4-3: Live mount the same snapshot again. And change the name of VM

With the get method which we used in the previous exercises you can control the api-call using parameters. 

With the post method you have to ‘post’ the data in what is called the payload which will be includied in the body of the api call. The payload is formated in JSON. 

In the example you can see how easy it is the generate the payload in the right (JSON) format.

![](/img/image105.png)

After mounting the VM check if the VM is mounted (in the GUI). Notice the name change of the mounted vm.

_This is an example that you have more option with the API than you have in GUI. It’s not possible to livemount a VM with another name using the GUI._

Unmount the vm in the gui. 
Copy and paste the curl command in the gui to see how the payload looks like in the GUI.

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: Basic <$token_id>' -d '{ \ 
   "vmName": "rubrik_api_training" \ 
  \ 
 }' 'https://<node-ip-address>/api/v1/vmware/vm/snapshot/ac723e1c-80a5-4dad-bbc3-eb7941017145/mount'
```

As you can see in above example , the curl command to excute the livemount can become pretty long and complex.

## Exercise 4-4 : Create an on-demand snapshot for a vm.

Get an object id for a vm for which you want snapshot on demand (see previous exercise for an example)

Create a on-demand snapshot for this VM using a different sla than the one which is assigned to that VM.  Hint: You can’t specify the name for SLA in the payload.

![](/img/image106.png)

Check in the gui if your on-demand snapshot has been started and executed. 
