# /screen/applicant/<purchase_token>  
  
**method:** GET,
**headers:** {"Authorization", "Token <your auth token>"}  
  
This is the url for retrieving detailed information about an applicant. This route is always available after you obtain your purchase token, and will contain what ever information we have access to. For instance if a tenant has filled out the first two pages of an application and no more, we will return whatever data we already have access to.  
**note:** when screening a tenant directly without sending them a form, much of this information will be null  

```
{
    "ten_first_name": "Tenant",
    "ten_middle_names": null,
    "ten_last_name": "Tenant",
    "ten_email": "example@email.com",
    "ten_tel": "5555555555",
    "ten_dob_day": "14",
    "ten_dob_month": "3",
    "ten_dob_year": "1979",
    "ten_address": "100 King Street West, Toronto, ON, Canada, M5X 1A9",
    "ten_previous_addresses": [
        "42 York Street, Ottawa, ON, Canada, K1N 5S6"
    ],
    "ten_employments": [
        {
            "job_title": "Barista",
            "employer": "Starbucks",
            "employment_length": "4 Years",
            "income": 55000
        }
    ],
    "ten_annual_income": 55000,
    "ten_household_income": 55000,
    "ten_pets": [
        {
            "type": "dog",
            "breed": "Golden Retriever"
        }
    ],
    "ten_automobiles": [
        {
            "make": "Volvo",
            "model": "240"
        }
    ],
    "ten_smoke": false,
    "ten_refused_to_pay_rent": false,
    "ten_bankruptcy": false,
    "ten_evicted": false,
    "ten_given_notice": false,
    "ten_additional_info": "I am a very quiet and pleasant person."
}
```
### The Response Object  
* "ten_first_name" - _tenant first name_  
* "ten_middle_names" - _tenant middle names. If there are multiple, they will be in one sting, for instance "jean michael"_  
* "ten_last_name" - _tenant last name_  
* "ten_email" - _tenant email_  
* "ten_tel" -  - _tenant phone number_  
* "ten_dob_day" - _tenant dob day_  
* "ten_dob_month" - _tenant dob month_  
* "ten_dob_year" - _tenant dob year_  
* "ten_address" - _the tenant's current address_  
* "ten_previous_addresses" - _a list containing any previous addresses the tenant provided. Each address in the list will be an address string. If none exist we return an empty string_
* "ten_employments" - _a list containing information about the jobs the tenant has listed on their application. If none exist we return an empty list_
* "ten_annual_income" - _the tenant's declared annual income_  
* "ten_household_income" - _the tenant's declared household income_  
* "ten_pets" - _a list containing any information about pets the tenant provided. If none exist we return an empty list_  
* "ten_automobiles" - _a list containing any information about automobiles the tenant provided. If none exist we return an empty list_  
* "ten_smoke" - _a boolean that indicates the self volunteered information whether the tenant smokes_  
* "ten_refused_to_pay_rent" - _a boolean that indicates the self volunteered information whether the tenant has ever refused to pay rent_
* "ten_bankruptcy" - _a boolean that indicates the self volunteered information whether the tenant has ever declared bankruptcy_
* "ten_evicted" - _a boolean that indicates the self volunteered information whether the tenant has ever been evicted_  
* "ten_given_notice" - _a boolean that indicates the self volunteered information whether the tenant give notice to their current landlord that they are moving_  
* "ten_additional_info" -  _any additional information that the tenant decided to include on the application as a note_