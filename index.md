

# API Reference

[Getting Started](GettingStarted.md)

[API Change Log](ChangesLog.md) 

[FAQ](FAQs.md)

[Fair Use Policy](FairUsePolicy.md)

#### The API reference documentation:

**[Lines API](https://pinny888.github.io/docs?api=lines)**

**[Bets API](https://pinny888.github.io/docs?api=bets)**

**[Customer API](https://pinny888.github.io/docs?api=customer)**


# Overview

#### Authentication 


The API uses HTTP Basic access authentication. Always use HTTPS to access the API.

You need to send HTTP Request header like this:
```
Authorization: Basic <Base64 value of UTF-8 encoded “username:password”> 
```

Example:

```
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ= 
```


Please note that in order to access Pinnacle888 API, you must have a funded account.

#### Data Formats 

Pinnacle888 API supports only JSON format.
HTTP header `Accept` must be set:
```
    Accept: application/json
```
POST HTTP Request must have JSON body content and `Content-Type` HTTP header must be set:

```
    Content-Type: application/json
```

#### Compression 

Pinnacle888 API supports HTTP compression. We strongly recommend using compression as it would give the best performance.

Please make sure to set the `User-Agent` HTTP header or compression might not work.

#### Date Time Format 

All dates and times are in GMT time zone, ISO 8601 format

#### Deduplication

When a client issues a network request, it is always possible for the request to timeout or return an error status code indicating that the bet may not have been accepted. This opens up the possibility of the same request being sent more than once, which could create duplicate bets. Deduplication is a technique to avoid creating these duplicates when retrying a create request. We do the deduplication automatically for you.  

If the bet is accepted, we store the `uniqueRequestId` in the system for 30 min. If you try again within that time range to place a bet with the same `uniqueRequestId`, you will get the appropriate error.

All place bet requests support deduplication.


# API Status
You can follow [Pinnacle888 status page](https://status.pinnacle888.com/) and subscribe to get the notifications on API status.  


**[Pinnacle888 API Open API Specification](https://github.com/pinny888/api-spec/blob/main/OpenAPI)** is hosted on GitHub.


# Disclaimer

 Pinnacle888 is not liable for use of the API for any purpose. The API is provided on an “as is” and “as available” basis, without warranties of any kind, either expressed or implied, including, without limitation, implied warranties of merchantability and fitness for a particular purpose or non-infringement.

 
