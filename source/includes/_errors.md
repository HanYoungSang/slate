# Errors


The CyberMDCare API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid. access-token is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The requested is forbidden.
404 | Not Found -- The request could not be found.
405 | Method Not Allowed -- You tried to access api with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
429 | Too Many Requests -- You're requesting too many kittens! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
