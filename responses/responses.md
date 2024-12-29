### Example response after initial request:

Method: **POST**
Route: /screen/embedded_flow_request or /api/request
Response Status Code: **200**
```  
{
    "purchase_token": "e8320e18365c64af5cc7db955ba6b350",
    "payment_status": "landlord has not submitted",
    "created": "2023-11-07T21:22:48.563110+00:00",
    "form_url": "http://localhost:8000/screen/request?purchase_token=e8320e18365c64af5cc7db955ba6b350"
}
```  


Example succesful response to Fetching a completed report:
Method: **GET**
Route: /screen/embedded_flow_get_report/<purchase_token> or /api/report/<purchase_token>
Response Status Code: **200**

```
{
    "purchase_token": "9826fd5d6a11edb47958460bc8af8a6c",
    "cost": 2249,
    "tax_collected": 292,
    "currency": "CAD",
    "data created": "Nov 01/2023, 01:17PM",
    "external_customer_id": "l2--l6",
    "landlord_email": "test@bu.com",
    "external_tenant_id": "te22--n6",
    "tenant_email": "tenant@email.com",
    "credit_card_last_4": null,
    "landlord_has_tenant_info": true,
    "tenant_pays": false,
    "embedded_flow_request": true,
    "api_only_purchase": false,
    "report_url": "https://sk-app-dev-s3-bucket.s3.amazonaws.com/platform2-reports-dev/report-6c08b0ac28fc52e449d3fabbbe27818d.pdf?AWSAccessKeyId=AKIAQZMAY4AHU7AT3CMD&Signature=s%2FVFMXUTlzTDp13pfkAWrSsENjU%3D&Expires=1699996899",
    "singlekey_score": null,
    "success": true
}
```

### Webhook Notifications


Example response when landlord has invited the tenant and the tenant hasn't responded (unopened email):
Method: **GET**
Route: /screen/embedded_flow_get_report/<purchase_token> or /api/report/<purchase_token>
Response Status Code: **200**

```
{
    "detail": "Tenant has not submitted invite. They have not opened their invite email.",
    "created": "2023-11-07T21:28:55.283146+00:00",
    "success": true,
    "form_url": "http://localhost:8000/screen/tenant?token=7E2RBE",
    "tenant_form_url": "http://localhost:8000/screen/tenant?token=7E2RBE"
}
```


Example response when landlord has invited the tenant and the tenant hasn't responded (opened email):
Method: **GET**
Route: /screen/embedded_flow_get_report/<purchase_token> or /api/report/<purchase_token>
Response Status Code: **200**

```
{
    "detail": "Tenant has not submitted invite. They have opened their invite email.",
    "created": "2023-11-07T21:28:55.283146+00:00",
    "success": true,
    "form_url": "http://localhost:8000/screen/tenant?token=7E2RBE",
    "tenant_form_url": "http://localhost:8000/screen/tenant?token=7E2RBE"
}
```


Example response when landlord has invited the tenant and the tenant has responded but the report is not yet ready:
Method: **GET**
Route: /screen/embedded_flow_get_report/<purchase_token> or /api/report/<purchase_token>
Response Status Code: **200**
```
{
    "detail": "Report creation in progress",
    "created": "2023-11-07T21:28:55.283146+00:00",
    "success": true
}
```


Example response when landlord has entered the tenant's information themselves (selfserve) but the report is not yet ready:
Method: **GET**
Route: /screen/embedded_flow_get_report/<purchase_token> or /api/report/<purchase_token>
Response Status Code: **200**
```
{
    "detail": "Report creation in progress",
    "created": "2023-11-07T21:28:55.283146+00:00",
    "success": true
}
```

Example response where there are errors in a submission that need to be fixed:
Method: **GET**
Route: /screen/embedded_flow_get_report/<purchase_token> or /api/report/<purchase_token>
Response Status Code: **200**
NOTE: *This will mark the tenant_form as unsubmitted so that the tenant can return to the form and fix the errors*
```
{
    "detail": [
        "1232133: SIN numbers must be 9 digits long"
    ],
    "created": "2023-11-07T21:28:55.283146+00:00",
    "success": false,
    "form_url": "http://localhost:8000/screen/tenant?token=7E2RBE",
    "tenant_form_url": "http://localhost:8000/screen/tenant?token=7E2RBE"
}
```

