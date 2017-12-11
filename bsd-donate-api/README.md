# BSD donate API version 1

## Response and request formats

### API Methods

This API currently supports two methods which is specified via the REST
endpoint URLs.

#### Charge

This API performs a charge on the contribution passed in providing a JSON API
for what normally happens through a BSD donation page. The endpoint URL is:

* `/page/cde/Api/Charge/v1`
* `/page/cde/Contribution/Api/v1` (deprecated)

#### Tokenize

This API tokenizes the contributor information and returns a raw gateway token.
This token can them be used to subsequently make a charge to the original card
or passed in to the `quick_donate_import` to create a Quick Donate record for a
constituent.

* `/page/cde/Api/Tokenize/v1`

#### Quick_donate_enroll

This API tokenizes the contributor information, enrolls the constituent into quick donate and returns 
constituent ID and the user ID. The constituent ID is the constituent's unique ID in our system. 
The user ID is the login ID that the constituent should use along with the password to sign in to
the system. Typically the user ID is the constituent's email address that was provided for this API.

* `/page/cde/Api/Quick_donate_enroll/v1`


### Request

Requests must be made with HTTPS POST.

REST endpoint URL: Varies depending on method. (See above.)

The POST params will be exactly the same as what is currently on BSD hosted
contribution pages (e.g.: https://DOMAIN/page/contribute).

We can only support SSL encrypted POSTs to the above endpoint due to PCI
requirements. Card numbers must never be sent via GET.

### Response

Responses will be in JSON format. JSONP is not supported for PCI and security
reasons.

### Arguments

To process a donation, send an HTTPS POST request to the endpoint mentioned
above with the following arguments.

*GET*

**lang**

* Charge: Optional
* Tokenize: Optional
* Quick_donate_enroll: Optional

Determines the language of the user facing messages in the response. Defualt
(i.e. if not specified) is english. For spanish, the value should be "es_MX"
for Mexican Spanish.

Note: The spanish translations for all the user facing messages from the BSD
donate API are stored as strings in the "dictionary fields" part of the BSD
control panel. These can be accessed here:
https://DOMAIN/utils/blue_dictionary/admin/section.php?component
=modules/contribution&section=Contribution%20Form%20Errors

*POST*

**slug**

* Charge: Required
* Tokenize: N/A
* Quick_donate_enroll: N/A

The slug of the BSD donation form under which the donation should be processed.

**submission_key**

* Charge: Optional
* Tokenize: N/A
* Quick_donate_enroll: N/A

A unique per page view key that the system will de-dupe on. If nothing is
submitted the dupe checker is disabled.

**firstname**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The user's first name.

**lastname**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The user's last name.

**password**

* Charge: N/A
* Tokenize: N/A
* Quick_donate_enroll: Required

Password for the user to sign in.

**addr1**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The first line of the user's billing address.

**addr2**

* Charge: Optional
* Tokenize: Optional
* Quick_donate_enroll: Optional

The second line of the user's billing address.

**city**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The city of the user's billing address.

**state_cd**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The state of the user's billing address.

**zip**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The zip or postal code of the user's billing address.

**country**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The country of the user's address. (US)

**email**

* Charge: Conditional
* Tokenize: Required
* Quick_donate_enroll: Required

The user's email address.

If this is set to required in the BSD donation form, then it should be required.

**phone**

* Charge: Conditional
* Tokenize: Required
* Quick_donate_enroll: Required

The user's phone number.

If this is set to required in the BSD donation form, then it should be required.

**amount**

* Charge: Required
* Tokenize: N/A
* Quick_donate_enroll: N/A

Because of the BSD payment processor this must be set to a value of "other" and
the amount must be passed as the amount_other argument mentioned below. When
using the donate API it is best practice to always set this as "other" and use
the amount_other argument (below) to specify the donation amount.

**amount_other**

* Charge: Required
* Tokenize: N/A
* Quick_donate_enroll: N/A

The donation amount selected by the user. Float value to the hundredth decimal
point. Examples: 10.00, 20.12

**quick_donate_populated**

* Charge: Optional
* Tokenize: N/A
* Quick_donate_enroll: N/A

The user's temporary, encoded payment token that is returned with the BSD
getToken JSONP endpoint. Required if no credit card information will be passed.

**cc_number**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The user's credit card number. If using a payment token the value should be the
last four of the user's credit card (retrieved from the getToken BSD endpoint).
Otherwise it should be the full credit card number.

**cc_type_cd**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The type of card from the user. If using a payment token the value should be
what is specified in the getToken endpoint. Otherwise it should be what the
user selects. Note that we have some JavaScript that does this detection
automatically so the user doesn't usually see an option to select credit card
type.

**cc_expir_month**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The expiration month of the user's credit card. If using a payment token the
value should be what is specified in the getToken endpoint. Otherwise it should
be what the user selects.

**cc_expir_year**

* Charge: Required
* Tokenize: Required
* Quick_donate_enroll: Required

The expiration year of the user's credit card. If using a payment token the
value should be what is specified in the getToken endpoint. Otherwise it should
be what the user selects.

**full_gift**

* Charge: Conditional
* Tokenize: N/A
* Quick_donate_enroll: N/A

Full gift is used if the contribution page has option for the donor to absorb the donation fees.
Only avaliable for Stripe Gateway.

**employer**

* Charge: Conditional
* Tokenize: Conditional
* Quick_donate_enroll: Conditional

The user's employer.

If this is set to required in the BSD donation form, then it should be
required. For the tokenization call the setting is the same as that of the
Quick Donate Enroll page.

Note: While you can set this to not being required in BSD, it may be a legal
requirement for your organization to always asks for this information.

**occupation**

* Charge: Conditional
* Tokenize: Conditional
* Quick_donate_enroll: Conditional

The user's occupation.

If this is set to required in the BSD donation form, then it should be
required. For the tokenization call the setting is the same as that of the
Quick Donate Enroll page.

**source_codes**

* Charge: Optional
* Tokenize: N/A
* Quick_donate_enroll: N/A

You can use this argument to pass a comma separated list of source or subsource
codes to the API.

**no_reporting_url**

* Charge: Optional
* Tokenize: N/A
* Quick_donate_enroll: N/A

When set to true, omits the td URL parameters form the success_url in the
response. These parameters contains the same tracking data included in the
response, but they are encoded. They are used to report contributions on
donation thank you page, but are sometimes not necessary for the Donate API.

**recurring_acknowledge**

* Charge: Required for recurring contributions
* Tokenize: N/A
* Quick_donate_enroll: N/A

To submit a recurring contribution you need to create a BSD donate page that is
set to type "recurring". Currently, you cannot have a recurring and
non-recurring BSD donate page. If POSTing to a BSD donate form slug that is a
recurring page, this parameter must be set to "1".

**custom1, custom2, custom3**

* Charge: Optional
* Tokenize: N/A
* Quick_donate_enroll: N/A

These are all fields to hold custom data. They don't do anything except hold
data associated with the donation in the BSD database.

**thank_you_override_url**

* Charge: Optional
* Tokenize: N/A
* Quick_donate_enroll: N/A

Instead of the preconfigured thank you page action this url will be returned as
the thank you redirect url when it is set. WARNING: This url is not validated
by BSD in any way and is simply passed through with "td" query strings appended
if applicable.

**ob_mailing_link_id**

* Charge: Optional
* Tokenize: N/A
* Quick_donate_enroll: N/A

Obfuscated mailing link ID, which can be retrieved from 'mlid' cookie. When passed 
on as parameter, the API will de-obfuscate the ID and embed it into the transaction record.

Note: the custom donation page must be on the same domain as the main donation page, 
otherwise the custom donation page would not be able to access the cookie.

**ob_mailing_recipient_id**

* Charge: Optional
* Tokenize: N/A
* Quick_donate_enroll: N/A

Obfuscated mailing recipient ID, which can be retrieved from 'mrid' cookie. When passed 
on as parameter, the API will de-obfuscate the ID and embed it into the transaction record.

Note: the custom donation page must be on the same domain as the main donation page, 
otherwise the custom donation page would not be able to access the cookie.

---

## Responses

### Charge Method

When the email address used for the donation is not already associated with a
saved payment token and the BSD form's "enable Quick Donate enroll process"
setting is enabled, the response will contain a cookie header that sets the
required cookies for the success url (which in this case
would be "Donation successful. Now, save your payment information.") to
function correctly.

A cookie for duplicate detection (`contribution_resubmission`) will also be set
on the response from the BSD donate API. This is used internally for BSD only.

**successful responses**

JSON status code: 200

The response will contain a JSON object with the API version number, reporting
data for the contribution as well as a success URL. The success URL contains
the page that the user should be redirected because of the successful donation.

**paypal redirect response**

JSON status code: 200

This response will contain a redirect url that the API consumer must redirect
the user to in order to continue the PayPal Express Checkout process. At the
time of the response the transaction has not been completed and may in fact be
aborted. If the transaction is successful the user will be redirected back to
BSD to complete the transaction, no notification will be sent to the JSON API
consumer with regard to the status of the contribution that it initiated.

**missing slug error response**

JSON status code: 400

This occurs when the slug is missing entirely. The response contains an API
version number, status ("fail"), and a failure code ("noslug"). We do not try
to contribute to the default contribution page in this instance as this is an
obvious development error.

**invalid slug error response**

JSON status code: 400

This occurs when the given slug does not exist. The response contains an API
version number, status ("fail"), and a failure code ("invalidslug"). We
currently do not try to contribute to the default contribution page, that may
happen in a later version of the API.

**validation failure response**

JSON status code: 400

This occurs when one of the user info. arguments in the request does not
validate correctly. (e.g. the user's email is malformed, there was no phone
number when that field is required, etc.) The response contains an API version
number, status ("fail"), a failure code ("validation"), and an array of errors.

**gateway error response**

JSON status code: 400

This occurs when the gateway declines the transaction. The response contains an
API version number, status ("fail"), a failure code ("gateway"), and a
`gateway_response` object that has more details passed back from the gateway
about the failure. That object will contain the following elements:

*  status - The status of the transaction this can be one of the following
   values: "error", "decline", "review", or "unknown"
*  code - The raw status code from the gateway for the failure
*  message - The corresponding error message for the given error code according
   to the API docs of the gateway
*  failed_avs - Whether AVS (address verification) check failed
*  failed_cvv - Whether CVV (card security code) check failed

**server error response**

JSON status code: 500

This occurs when the server is unable to process the request for any reason.
The response contains an API version number, status ("fail"), a failure code
("unhandled").

### Tokenize Method

**successful responses**

JSON status code: 200

The response will contain a JSON object with the API version number, the raw
gateway token, and a hash of contributor data contained within the token.

**validation failure response**

JSON status code: 400

This occurs when one of the user info. arguments in the request does not
validate correctly. (e.g. the user's email is malformed, there was no phone
number when that field is required, etc.) The response contains an API version
number, status ("fail"), a failure code ("validation"), and an array of errors.

**validation failure response**

JSON status code: 400

This occurs when we are not able to tokenize the payment information provided.
This is primarily due to the card being declined or being placed into the
review queue. The response contains an API version number, status ("fail"), and
a failure code ("notoken").

### Quick_donate_enroll Method

**successful responses**

JSON status code: 200

The response will contain a JSON object with the API version number, the status 
("success"), the constituent id ("cons_id") and user id ("user_id").

**create account failure response**

JSON status code: 400

This occurs when the password is missing, too short or an account has been enrolled with 
the same email and password. The response contains an API version number, status ("fail"), 
a failure code ("create_account"), and a description of the error ("description").


**validation failure response**

JSON status code: 400

This occurs when one of the user info. arguments in the request does not
validate correctly. (e.g. the user's email is malformed, there was no phone
number when that field is required, etc.) The response contains an API version
number, status ("fail"), a failure code ("validation"), and an array of errors.

**validation failure response**

JSON status code: 400

This occurs when we are not able to tokenize the payment information provided.
This is primarily due to the card being declined or being placed into the
review queue. The response contains an API version number, status ("fail"), and
a failure code ("notoken").
---

## Plans for version 2

*  Easier recurring payments. We'd like to have one BSD donate form accept both
   recurring and non-recurring payments.
*  Credit card type detection on the server side.
*  Unlimited custom fields
*  Easier amounts (not required two fields/POST vars)
*  Support for ticket fields
*  Grassroots fundraising support
*  Reduce BSD latency
