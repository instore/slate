# Errors

The Instore API uses standard HTTP status codes to report error conditions.  The response body will often contain specific detail on what is wrong, especially in the event of a 4xx response.  The most common responses are...

Error Code | Meaning
---------- | -------
400 | Bad Request -- The request is incorrect or illegal in some way.  Details in the response body.
401 | Unauthorized -- API key trouble.
403 | Forbidden -- This API key doesn't allow this activity.
500 | Internal Server Error -- A server problem.  A bug report was generated and sent to Instore.
503 | Service Unavailable -- Usually a response from our load balancer.  Try again after a short time.
