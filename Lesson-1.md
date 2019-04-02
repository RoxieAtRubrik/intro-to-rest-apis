# Lesson 1: Rubrik API Basics

The Rubrik CDM APIs are organized to conform the design principles of Representational State Transfer (REST). We also use the JSON data format for requests and responses. Whether you will be using our APIs directly to make your own custom integration or interacting with our SDKs, it's still worth taking the time to familiarize yourself on how REST APIs work and what they can do for you.

Our REST APIs are designed to be simple to understand and use, as well as be predictable with standard HTTP response codes for any and all API errors to allow you to interact securely with the API. Check out the API explorer for more information on specific endpoints, their parameters and response data formats.

Through authenticated and encrypted interaction with the Rubrik REST API server, perform any of the operations that are available through the Rubrik web UI and many bulk-type operations that might otherwise be difficult or impossible to perform.

A quick way to become familiar with the Rubrik REST API is to use the Rubrik REST API Explorer.

## Lesson 1-1: Rubrik CDM API Documentation

In Google Chrome, open a new tab and type `https://$cluster_address/docs/v1`. The node IP information can be found in [Lab Topology](/lab-topology.md).

The Rubrik RESTful API (v1) web page opens.

![API Docs](/img/image1-1.png)

Browse through the Rubrik CDM API documentation and note the different endpoints available to you.

| Note: Rubrik CDM API documentation is available online as well. You can find it [here](https://github.com/rubrikinc/api-documentation) |
| --- |

## Lesson 1-2: Authenticating Using API Explorer

In Google Chrome, open a new tab and type `https://$cluster_address/docs/v1/playground`. The node IP information can be found in [Lab Topology](/lab-topology.md).

The Rubrik RESTful API Explorer page opens.

![API Explorer-v1](/img/image1-2.png)

On the top-right hand of the page, click **Authorize**.

![Authorize](/img/image1-3.png)

The Available Authorizations page opens.

![Available Authorizations](/img/image1-4.png)

* **BasicAuth authentication** - uses the HTTP Basic Authentication method and requires you to include the user credentials with each API call. Since each API call made using the BasicAuth method is separately authenticated, you do not need to manage the session state. You also do not need to log out of a session, since this method does not create a session.

* **Token authentication** - creates a token at the beginning of a session and then uses that token to authenticate the API calls that are made during the session. The token remains valid for the session - normally 30 minutes after the last activity.

Enter the Rubrik CDM credential information found in [Lab Topology](/lab-topology.md).

Click **Authorize**.

## Lesson 1-3: Authenticating using cURL

In Google Chrome, open a new tab and type `https://$cluster_address/docs/v1/#section/Authentication/Authentication-session`. Review the chapter about authentication.

Generate a bearer token using the BasicAuth method using the cURL command:

`curl -k -u admin:pass -X POST "https://$cluster_address/api/v1/session"`

In this case, `username:password` is the username for an Admin account on the host Rubrik cluster, a colon, and the account password. For example, this command may resemble:

`curl -k -u admin:Rubrik123!! -X POST "https://192.168.2.150/api/v1/session"`

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
