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
## 2.8 Compare common API styles (REST, RPC, synchronous, and asynchronous)
## 2.9 Construct a Python script that calls a REST API using the requests library