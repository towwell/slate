# Errors

The Satsaco WMS API uses the following error codes. It can be used to assist in reforming the request body


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API credentials are wrong.
403 | Forbidden -- The data requested is for administrators only.
404 | Not Found -- The specified order/endpoint could not be found.
405 | Method Not Allowed -- You tried to access an API with an invalid method.
406 | Not Acceptable -- You requested a format that is not JSON.
410 | Gone -- API requested endpoint has been removed from our servers.
429 | Too Many Requests -- You're requesting too many orders! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
