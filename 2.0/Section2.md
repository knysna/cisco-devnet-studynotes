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

## 2.4 Explain common HTTP response codes associated with REST APIs
## 2.5 Troubleshoot a problem given the HTTP response code, request and API documentation
## 2.6 Identify the parts of an HTTP response (response code, headers, body)
## 2.7 Utilize common API authentication mechanisms: basic, custom token, and API keys
## 2.8 Compare common API styles (REST, RPC, synchronous, and asynchronous)
## 2.9 Construct a Python script that calls a REST API using the requests library