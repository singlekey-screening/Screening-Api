# Response Status Codes and Errors

## Successful requests:  
Successful requests always response with a **200** status code and will have a **JSON body** and headers that contain **{"Content-Type": "application/json"}**. The JSON payload will always contain the key-value pair **{"success": true}**. For examples, see [Singlekey Screening Responses](https://github.com/singlekey-screening/Screening-Api/blob/main/responses/responses.md)  
  
## Unsuccessful requests:  
With the exception of 500 errors that do not have a JSON body, all unsuccessful requests will have a JSON body that contains the key-value pair **{"success": false}**, and the keys ["detail", "errors"], where "detail" is a string with a description of the error and "errors" is a list of problems with the request. For example, if you make a POST requests to /screen/embedded_flow_request with multiple emails that are improperly formatted you will receive a 400 response like  
```
{
    "success": false,
    "detail": "missing or incomplete data",
    "errors": [
        "ten_email: the email --no_at_sign.com-- is not formatted like an email",
        "ll_email: the email --no_domain@email-- is not formatted like an email"
    ]
}
```  
The "errors" should be considered appropriate to display to your users, so they can fix any mis-formatted information if possible.  
  
## Status Errors:  
  
### 500

There are two types of 500 status codes you may receive:  

- 500 errors ** with no JSON body** and headers that contain **{"Content-Type": "text/html;charset=UTF-8"}**. These are HTML responses that very likely means **your IP address has been blocked by Cloudflare or our AWS WAF**. This is often due to requests coming from a geographic location known for bad actors, although your IP address may be flagged for other reasons.  In this case, please contact us and we will *"allow list"* your server's IP address.  
- 500 errors **with a JSON body** and headers that contain **{"Content-Type": "application/json"}**. These indicate an internal server error that is handled but is preventing the normal operation of our service. In this case, please contact us and include the JSON response and we will fix the error as quickly as possible.  
  
All responses that are not 500 errors will be JSON responses and will include the key-value pair indicating weather or not the request was successful such as **{"success": true}** or **{"success": false}**, with the exception of **401** authorization errors which do not contain that flag.
  
### 401  
A 401 error indicates that you either did not include Authentication credentials in the headers of your request, or your credentials are incorrect.  
```
{
    "detail": "Invalid token."
}  
```  
Please ensure you are including your correct auth_token in the headers for your request: **{"Authorization": "Token <your auth token>"}**  
  
### 404  
A 404 response indicate that the resource you are looking for does not exist. This likely means that you are using an incorrect token to retrieve a report.  
```
{
    "detail": "Report does not exist",
    "errors": [
        "Report does not exist"
    ],
    "success": false
}
```  
Please check that you are using the correct purchase_token to retrieve a report.  
  
 ### 400  
 A request that returns a response with a **400** status code will have a JSON payload that includes the key-value pair **{"success": false}** and indicates your requests was correctly formatted request but is either missing required information or has improperly formatted data. For instance if you submit a request with a "ten_sin" included in the payload, but the SIN is only 5 digits, you will receive a response with a **200** status_code, but the following error:
 ```
 {
    "success": false,
    "detail": "missing or incomplete data",
    "errors": [
        "12345: SIN numbers must be 9 digits long"
    ]
}
```  
If possible, these errors can be displayed to your uses so that they can correct or remove the offending data.  

