# 2.0 Understanding and Using APIs

## 2.1 Construct a REST API request to accomplish a task given API documentation
I assume this will be a multiple choice question with an exhibit    

* Understand HTTP Methods: GET POST PATCH DELETE etc. (just the most common ones)
    * and how they relate to CRUD concept (Create, Read, Update, Delete)
* Understand URLs and API endpoints
* Service API's vs Informational APIs
* HTTP/1.0 or HTTP/1.1
* From the supplied doc you will see the required headers, if body is mandatory, etc.

## 2.2 Describe common usage patterns related to webhooks
* Sometimes called 'reverse API'
* Webhooks are used to send information about some triggering event to an HTTP endpoint (as opposed to you querying the API about any possible events)
* Requires a token else the webserver will drop the message - unique token generated per webhook device

## 2.3 Identify the constraints when consuming APIs
* [Fielding’s constraints](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
* Specifically constraints for REST APIs
* Client/server
    * Must be completely independent from each other
* Stateless
    * Every request completely self contained (concept of authentication tokens breaks this?)
* Cache
    * Client should cache previous responses and avoid sending the same request
    * e.g if previosu request contains a cache time, don't request again until that timer expired
* Uniform interface
    * URIs do not change
    * Manipulation of resources through representation. Representation =/= the resource itself
    * Self-descriptive messages that contain all the required info. Headers, Body, Method.
    * Hypermedia as the Engine of Application State (HATEOAS)
        * If Hypertext = text then Hypermedia = other types of media
* Layered system
    * The layers refer to modular, swappable things between the client and server that must not interfere with the request
    * e.g. Proxy, Load balancer
    * Could also be layers beind the server e.g. database
* Code on demand
    * Optional constraint
    * If server sends code (e.g. Java, Flash) to client, the client needs to know how to process it, hence it is optional.

## 2.4 Explain common HTTP response codes associated with REST APIs
* https://restfulapi.net/http-status-codes/
* https://tools.ietf.org/html/rfc7231

### Codes Overview
* 1xx: Informational – Communicates transfer protocol-level information.
* 2xx: Success – Indicates that the client’s request was accepted successfully.
* 3xx: Redirection – Indicates that the client must take some additional action in order to complete their request.
* 4xx: Client Error – This category of error status codes points the finger at clients.
* 5xx: Server Error – The server takes responsibility for these error status codes. 
### Most common codes
* 200 (OK)
* 401 (Unauthorized) - check that the credentials used are correct
* 403 (Forbidden) - you did everything correctly... but you don't have permission for that action
* 404 (Not Found)
* 500 (Internal Server Error) - unspecified, but definitely not the client's fault
* 502 (Bad gateway) - *not* referring to the client's IP gateway. Issue is some back end service that the server is trying to reach


## 2.5 Troubleshoot a problem given the HTTP response code, request and API documentation
Not much to note here...just putting all of the theory into practice.    
I expect that the question will be a multiple choice with several exhibits.


## 2.6 Identify the parts of an HTTP response (response code, headers, body)
It is handy to practice in Postman against any API and look at the Raw responses.    
Many free practice APIs at https://apilist.fun/.    
In Postman besides the nice default view, also look at Console -> Show raw log.
```
GET /facts HTTP/1.1
User-Agent: PostmanRuntime/7.26.10
Accept: */*
Postman-Token: c8699b26-cb57-4cb2-8ed2-27656d2dd8df
Host: cat-fact.herokuapp.com
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
HTTP/1.1 200 OK
Server: Cowboy
Connection: keep-alive
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 1859
Etag: W/"743-qKJxhXyFfDm3UacChEXm2NiUA/4"
Set-Cookie: connect.sid=s%3AAccp8-mEOb1ZGPuFh5H652O1UB-kCSYa.EmtwTAro%2Fyw7LJr1TzTx%2F4ND4Atp1jkB7hKuPC9K1PM; Path=/; HttpOnly
Date: Thu, 04 Mar 2021 08:41:59 GMT
Via: 1.1 vegur
[{"status":{"verified":true,"sentCount":1},"type":"cat","deleted":false,"_id":"58e008800aac31001185ed07","user":"58e007480aac31001185ecef","text":"Wikipedia has a recording of a cat meowing, because why not?","__v":0,"source":"user","updatedAt":"2020-08-23T20:20:01.611Z","createdAt":"2018-03-06T21:20:03.505Z","used":false},{"status":{"verified":true,"sentCount":1},"type":"cat","deleted":false,"_id":"58e008630aac31001185ed01","user":"58e007480aac31001185ecef","text":"When cats grimace, they are usually \"taste-scenting.\" They have an extra organ that, with some breathing control, allows the cats to taste-sense the air.","__v":0,"source":"user","updatedAt":"2020-08-23T20:20:01.611Z","createdAt":"2018-02-07T21:20:02.903Z","used":false},{"status":{"verified":true,"sentCount":1},"type":"cat","deleted":false,"_id":"58e00a090aac31001185ed16","user":"58e007480aac31001185ecef","text":"Cats make more than 100 different sounds whereas dogs make around 10.","__v":0,"source":"user","updatedAt":"2020-08-23T20:20:01.611Z","createdAt":"2018-02-11T21:20:03.745Z","used":false},{"status":{"verified":true,"sentCount":1},"type":"cat","deleted":false,"_id":"58e009390aac31001185ed10","user":"58e007480aac31001185ecef","text":"Most cats are lactose intolerant, and milk can cause painful stomach cramps and diarrhea. It's best to forego the milk and just give your cat the standard: clean, cool drinking water.","__v":0,"source":"user","updatedAt":"2020-08-23T20:20:01.611Z","createdAt":"2018-03-04T21:20:02.979Z","used":false},{"status":{"verified":true,"sentCount":1},"type":"cat","deleted":false,"_id":"58e008780aac31001185ed05","user":"58e007480aac31001185ecef","text":"Owning a cat can reduce the risk of stroke and heart attack by a third.","__v":0,"source":"user","updatedAt":"2020-08-23T20:20:01.611Z","createdAt":"2018-03-29T20:20:03.844Z","used":false}]
```

## 2.7 Utilize common API authentication mechanisms: basic, custom token, and API keys
### HTTP Basic Authentication
* Password encoded in Base64, passed in a header in the HTTP request 
* If used in plain HTTP, the password is visible in every request. Must use HTTPS.
* Very commonly used, even by some Cisco security products

### Custom Token
* A user is initially authenticated (e.g. using Basic Authentication) and presented a unique token
* That token is used in subsequent requests instead of the user's own password
* Token can be set to expire
* Token can be refreshed using a refresh token
* Token can be cached by user's web browser for the duration of the session

### API Keys
* API keys are pre-shared keys known to the server and client
* API keys can be included in requests by means of cookies, request headers, strings in the body of a request
* The key is sent with every API call
* Typically issued per-user/per-application and can be revoked

## 2.8 Compare common API styles (REST, RPC, synchronous, and asynchronous)
* REST and RPC both use HTTP, both need a verb/method and a resource/endpoint
* Plenty of debate online about which is 'better'
### RPC - Remote Procedure Call
* Best suited for actions: execute this process
* XML-RPC has all the advantages and disadvantages of XML
### REST - Representational State Transfer
* Best suited for CRUD operations
### Synchronous
* Send a request, get the response containing the answer immediately
* The application must wait for the response before it can continue processing
    * Can result in interruptions/delays
### Asynchronous
* Send a request, get a response acknowledging the request immediately, get a response containing the answer at some later time
* The application does not need to wait for the response before it can continue processing
    * Can result in faster applications
* Example: requesting a report that will take the server a long time to run
* Can be an additional, separate API to query the status of asynchronous requests e.g. Cisco DNA Center's 'Task API'

## 2.9 Construct a Python script that calls a REST API using the requests library
### How to:
* Import the needed libraries e.g. requests, json
* Define the API's base URL and the specific API endpoint URI
* Define authentication/token/API key as required
* Construct the HTTP headers as described by the API documentation
* Construct the HTTP body as described by the API documentation (if a body is required - would be useed for POST more than GET)
* Submit the call using the requests library and the correct method.
* Do something with the response e.g. print it


### Example GET request from Meraki API:
```
import requests
import json

meraki_tenantID = "01234567899876543321"

meraki_api_key = "1a2b3c4d5e6f0000111112222"
url =  "https://api.meraki.com/api/v0/organizations/"
devices_url = url+meraki_tenantID+"/devices"

headers = {
        "X-Cisco-Meraki-API-Key": meraki_api_key,
    }
params = {
    "perPage": 5
}

orgs = requests.get(url,headers=headers)
devices = requests.get(devices_url, headers=headers, params=params)

print(orgs)
print(devices)
```