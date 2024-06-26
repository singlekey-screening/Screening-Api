# Screening-Api
SingleKey Screening Api offers a fast, secure solution for landlords and property managers to screen applicants, providing full Equifax or Transunion credit data and background screening information in under 5 minutes.  

For a sample of our report or to learn more about our services, see [singlekey.com](https://singlekey.com), or contact us at **info@singlekey.com** and **1 (877) 978-1404**  
To get started with credentials for our sandbox environment, contact us at **info@singlekey.com**

# Overview
SingleKey offers two easy ways to integrate our screening functionality into your flow:  

### Embedded Flow
**Use our forms to collect your customer's data.**  
Your user can request a new screening from inside of your portal. This includes the ability for your users to use both of our [Screening Flows](#screening-flows). Your server will requests a new screening from our API, which will return a unique link where your users can fill out their information and request a tenant screening. That unique form can be pre-populated with data that you provide in the initial request. After they are finished they will be redirected back to your portal. You will be notified by webhook after the screening is complete and you will be provided a link to the report. Payment can be authorized by your application or made directly by your user.  
### Pure API Access   
**Provide your user's data directly throught the API**  
When your user requests a tenant screening, your server will send a request to our API with all required data to initiate a screening. You will be returned a token that will allow you to fetch your report when it is finished. You will be notified by webhook after the screening is complete and you will be provided a link to the report. A payment method must be added to your account via the API payments method do activate this feature.  

# Authentication  
Requests are authenticated by Token Authentication. You will provide your token in the headers for all of your request:  
``{"Authorization": "Token <your_token>"}``  

# Environments  
All production requests are made to: <https://platform.singlekey.com>  
All sandbox requests are made to: <https://sandbox.singlekey.com>  

# Making A Request    

**A full list of fields which can be submitted can be found in this repository at /fields/available_fields.txt**  
**Some example payloads can be found in this repository at /example_payloads**  

### Embedded Flow  

Using the embedded flow offers two options:  
-Obtain a form to the landlord, who can choose to either invite a tenant to fill out an application via email and text, or (provided they have proof of consent and required information) screen the tenant immediately  
-Obtain a form to present directly to the tenant. Do do this, include the optional key-value pair {"tenant_form": true} in the json payload  
   
Before making an embedded flow requests we require three pieces of information from you:  
**redirect_url:** A url to redirect your users back to your portal after they have finished requesting their tenant screening.  
**notification_callback_url:** A url for our webhook to notify you when status changes occur with your request.  
We also can configure an email or text alert to notify you if our request to send a webhook is unsuccessful (does not receive the expected response status code, usually 200)  
To receive that notification you can provide us with an **alert_email** and  **alert_text**  
  
Optionally, you can also provide and image of your **branding** so that it can be include on our forms.  

**Required Json Fields:**  
&nbsp;&nbsp;'*ll_first_name*': string - landlord's first name  
&nbsp;&nbsp;'*ll_last_name*': string - landlord's last name  
&nbsp;&nbsp;'*ll_tel*': string - landlord's phone number  
&nbsp;&nbsp;'*ll_email*': string - landlord's email   
&nbsp;&nbsp;'*purchase_address*' - the address of the unit the applicant is applying to  

To obtain the tenant application form directly include:  
&nbsp;&nbsp;'*tenant_form*' - true
  
To initiate a screening, make a POST request to **/screen/embedded_flow_request** with the required authentication and information.  
Example:  
``curl -i -H "Authorization: Token <your_token>" -H "Content-Type: application/json"  --request POST <target_environment>/screen/embedded_flow_request --data '{"ll_first_name":"Example","external_customer_id":<your_id_for_user>, ...<required_data>... }'``  

Response:  
``{``
&nbsp;&nbsp;``"purchase_token": "0f9a323bac7776a301d4b260c8be4e04",``  
&nbsp;&nbsp;``"payment_status": "landlord has not submitted",``  
&nbsp;&nbsp;``"created": "2024-03-30T14:08:53.215066+00:00",``  
&nbsp;&nbsp;``"form_url": "http://localhost:8000/screen/request?purchase_token=0f9a323bac7776a301d4b260c8be4e04"``  
``}``  
  
### Pure API Request  
  
To initiate a pure API screening, make a POST request to **/api/request** with the required authentication and information.  
Example:  
``curl -i -H "Authorization: Token <your_token>" -H "Content-Type: application/json"  --request POST <target_environment>/api/request --data '{"ll_first_name":"Example","external_customer_id":<your_id_for_user>, ...<required_data>... }'``  

Response:  
``{``
&nbsp;&nbsp;``"purchase_token": "e9227009cacde4446f4d508d090fef02",``  
&nbsp;&nbsp;``"payment_status": "paid",``  
&nbsp;&nbsp;``"created": "2024-03-30T14:13:03.109200+00:00",``  
&nbsp;&nbsp;``"initiated": true``  
``}``  
  

**Required Json Fields:**  
'*ll_first_name*': string - landlord's first name  
'*ll_last_name*': string - landlord's last name  
'*ll_tel*': string - landlord's phone number  
'*ll_email*': string - landlord's email  
'*purchase_address*' - the address of the unit the applicant is applying to
'*run_now*': boolean - true to run the report immediately  
‘*ten_first_name*’: string - tenant’s first name  
‘*ten_last_name*’: string - tenant’s last name
‘*ten_email*’: string - tenant’s email  
‘*ten_tel*’: string - tenant’s phone number, format with dashes or brackets.  
‘*ten_dob_day*': integer - tenant’s date of birth day. 1 or 2 digits acceptable  
‘*ten_dob_month*’: integer - tenant’s date of birth month. 1 or 2 digits acceptable  
‘*ten_dob_year*’: integer - tenant’s date of birth year. 4 digits required  
‘*ten_sin*’: integer - tenant’s social insurance number. 9 digits required **(Only required when screening an applicant who's primary residence is in the US)**  
'*ten_address*': string - tenant's current address  
&nbsp;&nbsp;&nbsp;&nbsp;!format! "street address, city, province two letter abbreviation, country"  
&nbsp;&nbsp;&nbsp;&nbsp;!This is important!. We use google address search to format all addresses for our automated application process.  
 

# Fetching a Completed Report:  
After your report is complete, you will be notified by webhook that you can fetch your report using your authentication and the **purchase_token** you received as a response when you ordered the report.  

To fetch the report created through the **embedded flow**, make a GET request to **/screen/embedded_flow_get_report/<purchase_token>** with the required authentication.  

To fetch the report created through the **pure api flow**, make a GET request to **/api/report/<purchase_token>** with the required authentication.  

Example:  
``curl -i -H "Authorization: Token <your_token>" --request GET <target_environment>/screen/embedded_flow_get_report/<purchase_token>``  
**or**  
``curl -i -H "Authorization: Token <your_token>" --request GET <target_environment>/api/report/<purchase_token>``  

Response:  
**if in progress:**  
``{``  
&nbsp;&nbsp;``"detail: 'Report creation in progress'``  
``}``  
  
**if report is complete:**  
``{``  
&nbsp;&nbsp;``"cost": Integer,``  
&nbsp;&nbsp;``"data created": Datetime String,``  
&nbsp;&nbsp;``"external_customer_id": String,``  
&nbsp;&nbsp;``"external_tenant_id": String,``  
&nbsp;&nbsp;``"credit_card_last_4": String,``  
&nbsp;&nbsp;``"landlord_has_tenant_info": Boolean,``  
&nbsp;&nbsp;``"report_url": String``  
``}``
  
# Re-running a report / Persisting a tenant's information for multiple landlords  
You can identify your tenants to us by including an external_tenant_id in your request. This is a unique id created by you that you can use to identify a particular tenant.  
  
This is useful when there is the possibility that you have data on one tenant who will be applying to multiple landlords(users) within your system. In this case, after one of your landlord(user) screens a tenant, the recent credit data we have on the tenant will be returned to any of your other landlords(users) who screen that tenant for up to thirty days after the last screening. This prevents you from being charged multiple times for running a credit report on a tenant who's information we have recently run. After 30 days, we consider that report no longer valid, as the tenants credit history may have changed.  
  
If you would like to rerun a tenant's information with updated data within the thirty day window you can pass the value {"update": true} as part of your request. We will run a new credit check with the updated information. This is useful when your initial request contained incorrect information that led to a bad result on the initial report. This will generate a new purchase token  
  
# Payments
You can tie a payment to your account using the by sending your credit card information to **/screen/payments**  

To add a credit card to your account post your credit card details  
``payload = {``  
&nbsp;&nbsp;``"card_number": String,``  
&nbsp;&nbsp;``"card_expiry_month": Integer,``  
&nbsp;&nbsp;``"card_expiry_year": Integer,``  
&nbsp;&nbsp;``"card_cvc": Integer,``  
&nbsp;&nbsp;``"card_postal": String``  
``}``  

``curl -i -H "Authorization: Token <your_token>" -H "Content-Type: application/json"  --request POST <target_environment>/api/request --data <payload>``  

Response:
``{``  
&nbsp;&nbsp;``"detail": "Payment added"``  
``}``  

You can check the status of your payment method by making a post request to **/api/payments** with your authentication  
Example:  
``curl -i -H "Authorization: Token <your_token>" --request POST <target_environment>/api/payments``  

Response for accounts with authorized payments:  
``{``  
&nbsp;&nbsp;``"detail": "payment authorized",``  
&nbsp;&nbsp;``"card_details": {``  
&nbsp;&nbsp;&nbsp;&nbsp;``"brand": "Visa",``  
&nbsp;&nbsp;&nbsp;&nbsp;``"last4": "4242",``  
&nbsp;&nbsp;&nbsp;&nbsp;``"expiry": "2/2022"``  
&nbsp;&nbsp;``}``  
``}``  

# Screening Flows  
When using our forms, users have two options for screening tenants. Please see /screenshots for examples:  
### Invite Tenant  
Users fill out basic information about the tenant like name, email and phone number, and we send an application to the tenant so they can provide the rest of the information we require to run the background check, and additional optional information such as references and documents. Users can make the inclusion of certain information mandatory if they desire.  The report is run immediately upon the tenant's submission. 
### Self Serve  
If a user has the required information and **the tenant's consent**, they can run the report immediately. Users can upload their consent document and the tenant's id if available, and the report is run upon submission.

# Responses  
To each request you make to our server, we will respond with a json payload that includes information on the status of your request, or detailed error information if applicable. Most of these responses will include:  
- your token  
- your request status  
- a from url you can use to update the submission  
- the tenant application link. Even if you are making a pure api request, you can use the tenant form to update your application  
- any relevant error information  
  
**A partial list of responses can be found in this repository at /responses/responses.txt**  
  
# Admin Portal  
When we issue you your credentials we will also give you access to an admin portal that you can use to perform many actions such as:  
- checking the status of your application  
- resending invites  
- identifying errors with a submission  
  

