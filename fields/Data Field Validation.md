# Api Data Field Validation

This is information on the field validation for all of the properties that are required to submit to Equifax Canada using the Singlekey API.

If a request is submitted that violates any of the information in the Notes section, the request will fail and the response will contain a description of the error. For instance, if a phone number is too short, your response will look like:  
{  
   "success": **false**,  
   "detail": "missing or incomplete data",  
   "errors": \[  
       "ten\_tel: the phone number \--555555-- is not the expected length"  
   \]  
}

## Names:

Relevant fields:

* ten\_first\_name  
* ten\_last\_name  
* ll\_first\_name  
* ll\_last\_name

Notes:

* These values are strings that must be between 1 and 256 Characters in length  
* Any UTF-8 character is acceptable  
*  You may not submit a null value, empty string or other falsy values

## Emails:

Relevant fields:

* ten\_email  
* ll\_email


Notes:

* Emails must include an @ sign  
* Emails must have characters in front of the @ sign (“@example.com” is not acceptable)  
* Emails must have a domain name at the end of the email address (“example@email” is not acceptable)  
* Email domains must be between 2 and 5 characters (“example@email.c” and “[example@email.longdomainname](mailto:example@email.longdomainname)” are not acceptable)

## Phone Numbers:

Relevant fields:

* ten\_tel  
* ll\_tel

Notes:

* Telephone numbers must be string, not an integer  
* Telephone numbers must contain **either** 10 numeric characters in length **or** 11 numeric characters in length *if they have a leading “1”*. **We strip leading 1’s before saving the number to our database, so a 10 character number starting with 1 will be invalid** (“1-555-555-5555” is acceptable, “155-555-5555” is not acceptable)  
* Only numeric values count toward the length of the number (“(555)(555)” is considered to be 6 characters in length)  
* You may include round or square brackets and hyphens (“555-555-5555”, “\[555\]5555555” and “(555)555-5555” are acceptable)  
* Telephone numbers must contain the area code. All North American phone numbers are 10 digits in length and include a 3 digit area code (“123-1234” is not acceptable)

## Date Fields:

Relevant fields:

* ten\_dob\_year  
* ten\_dob\_month  
* ten\_dob\_day

Notes:

* Date fields are type string, but integers will also submit (1, “01” and “1” are all acceptable)  
* Years must be 4 characters in length  
* Applicants must be born after 1900  
* Applicants must be over 18 years of age (an applicant who is 17 and 360 days old may not be screened)  
* Dates must be valid (“2000-06-32” and “2000-14-01” are not acceptable)

## Addresses

Relevant fields:

* ten\_address  
* purchase\_addresss

Notes:

* ten\_address must have at least one comma in the submitted value (“100 Yonge Street, Toronto” is acceptable, “100 Yonge Street Toronto” is not)  
* Addresses that are not in either the US or Canada will not submit  
* **We use the Google address API to validate addresses**, so many violations of the rules below will be acceptable. If you submit an address that is not formatted as we expect, we feed it to google and this will often produce the correct result. For instance, if you submit the address “100 Young Street, Toronto”, we will use the Google API to convert that to “100 Young St., Toronto, ON, Canada, M5B 2H6”  
* That being said, Google can falsely identify an address if you do not provide enough information, so try to adhere to the following best practices:  
  * Addresses are a string with 5 comma separated components. “Street and street number”, “city”, “province/state”, “country”, “zip/postal code”. For example “123 Fake St, Baltimore, MD, USA, 21201” (“123 Fake St-Baltimore-MD-USA-2120” and “Baltimore, 123 Fake St, MD, USA, 2120” are not acceptable)  
  * Provinces/states should be the correct two letter abbreviations. (“ON” is preferred to “Ontario”)  
  * The county should either be “Canada” or “USA”  
  * Canadian Postal Codes are always 6 characters, starting with a letter and alternating between letters and numbers (“M6E 1E7” is valid. “LL0 22E” is not a valid postal code)  
  * It is optional to include the space in the middle of Canadian postal codes  
  * US Zip Codes are 5 numbers in length

## Other Fields

All other fields are optional and may be excluded or submitted as null values.

If included:

* **Integers** must be between \-2,100,000,000 and \+2,100,000,000  
* **Strings** must be between 1 and 256 characters in length