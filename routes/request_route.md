# /screen/embedded_flow_request  
  
**method:** POST,
**headers:** {"Content-Type": "application/json", "Authorization", "Token <your auth token>"}  
  
This is the url for creating a screening request. There are three types of requests that can be made:  
  
1. Request a landlord form  
2. Request a tenant form  
3. Request an immediate screening  

All types of requests will return a purchase_token that you will use to identify the screening in the future.  

## 1.Request a landlord form  
This flow allows you retrieve a form that you can redirect your landlords to. Once on that screen the landlord is presented with the option of either:
* Sending an invite to the tenant to fill out the application
* Initiating a screening instantly, provided they have the required information and the tenant's consent  
  
the minimum data to initiate a request with this flow is below, but you may submit any information in [available fields](https://github.com/singlekey-screening/Screening-Api/blob/main/fields/available_fields.txt)
### Example request payload:
```
{
    "ll_first_name": "Landlord",
    "ll_last_name": "Landlord",
    "ll_email": "ll@email.com",
    "ten_first_name": "Tenant",
    "ten_last_name": "Tenant",
    "ten_email"
}
```  
**please note** the landlord will be required to enter other information about the tenant and the property they are renting out before they can submit the form.

### Example response  
```
{
    "success": true,
    "purchase_token": "****Unique_32_Character_Token***",
    "payment_status": "landlord has not submitted",
    "created": "2025-01-09T19:40:26.632576+00:00",
    "form_url": "https://platform.singlekey.com/screen/request?purchase_token=****Unique_32_Character_Token***"
}
```  
  
## 2.Request a tenant form  

This flow allows you retrieve a form that you can redirect or email your tenant applicants to. Depending on your preference/agreement with SingleKey, the tenant may be required to provide payment before submitting. You must include the property ```{"tenant_form": true}``` in your request.
  
   **please note** We require the purchase_address (rental address) in order to determine the jurisdiction of the transaction when using this flow.
   
### Example request  
```
{
"ll_first_name": "Landlord",
"ll_last_name": "Landlord",
"ll_email": "landlord@email.com",
"ten_first_name": "Tenant",
"ten_last_name": "Tenant",
"ten_email": "tenant@email.com",
"purchase_address": "14 York st, Toronto, ON, Canada, M5J 2Z2",
"tenant_form": true
}
```  
  
### Example response  
```
{
    "success": true,
    "purchase_token": "****Unique_32_Character_Token***",
    "payment_status": "on hold, payment will be captured on submission",
    "created": "2025-01-09T19:56:09.920818+00:00",
    "tenant_form": true,
    "form_url": "https://app.singlekey.com/t/listings/<hashed_id>/applications/new?tenant_token=<hashed_id>"
}
```  
  
## 2.Request an immediate screening  
  
 This flow allows you to immediately screen an applicant, provided you have all of the required information and consent from the tenant. In this flow, there is no forms involved. You send SingleKey the required data, and we process the report immediately. In this flow you must include the property ```{"run_now": true}``` in your request.
   
### Example request  
```
{
    "ll_first_name": "Landlord",
    "ll_last_name": "Landlord",
    "ll_email": "landlord@email.com",
    "ten_first_name": "Tenant",
    "ten_last_name": "Tenant",
    "ten_email": "tenant@email.com",
    "ten_address": "14 York st, Toronto, ON, Canada, M5J 2Z2",
    "ten_dob_year": "1980",
    "ten_dob_month": "1",
    "ten_dob_day": "14",
    "purchase_address": "14 York st, Toronto, ON, Canada, M5J 2Z2",
    "run_now": true
}
```  
  
### Example response  
```
{
    "success": true,
    "purchase_token": "****Unique_32_Character_Token***",
    "payment_status": "paid",
    "created": "2025-01-09T20:01:58.642255+00:00",
    "initiated": true
}
```

### The Response Object  
The response will always contain a unique 32 character **"purchase_token"**. This should be saved to your database as a reference to the screening.  

The response will also contain a **"payment_status"**. This will be one of the following:
1. "paid" - _we consider this to be paid, either by immediate billing or another arrangement such as monthyly billing or account credits_  
2. "landlord has not submitted" - _indicates that you have obtained a landlord form and the landlord has not submitted the form. No screening has been initiated at this status_  
3. "on hold, payment will be captured on submission" - _indicates that the landlord has used his form to invite a tenant and the landlord has provided payment, however the payment will not be captured until the tenant submits the application. This hold will be kept for 5 days and released at that point if the tenant has not responded_  
4. "unpaid" - _indicates that no payment has been received for this request. This is likely occurs when an invite requiring payment has been sent to a tenant, and they have not yet provided payment.  
  
