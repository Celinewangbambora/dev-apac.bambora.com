---
title: dev/bambora

language_tabs:
  - shell: cURL
  - html : Response

toc_above:
- <a href='index.html'>Getting Started</a>
- <a href='checkout.html'>Checkout</a>

includes:

toc_below:
- <a href='apis.html'>APIs</a>
- <a href='backoffice.html'>Backoffice</a>
- <a href='testing.html'>Testing</a>
- <a href='migration.html'>Migration to Production</a>

search: false
---
<script src='js/vendor/clipboard.min.js'></script>
<script src='js/copy.js'></script>

#Overview

Bambora have developed a flexible and innovative platform called Checkout for delivering payments online which is device agnostic, with responsive functionality over web, tablet and all other mobile devices

This guide will provide you with all information you need in order to get started.
First you should identify how you would to include the Checkout in your business.      

There are two options:

* Integrated
* Standalone

#Integrated

Checkout Integrated allows you to embed the payment page within your website through an iFrame. This enables your customers to make payment on your website and allow you to provide a seamless integration with Bambora through a notification of the transaction result returned back to your internal system.

##1. Create a session

Firstly, the POST generated by your server is submitted to the Bambora server
URL: [https://demo.bambora.co.nz/access/index.aspx](https://demo.bambora.co.nz/access/index.aspx)

```shell
cURL "https://demo.bambora.co.nz/access/index.aspx"
-d 'UserName=[API Username]'
-d 'password=[API Password]'
-d 'CustRef=[reference]'
-d 'Amount=[amount value]'
-d 'SessionId=[SessionId]'
-d 'SessionKey=[SessionKey]'
-d 'dl=checkout_V1_IHPP_Purchase'
-d 'ServerURL=[ServerURL]'
-d 'UserURL=[UserURL]'
-d 'AccountNumber=[Account Number]'
```

```html
<html>
  <body>
    <form>
      <input type="hidden" name="SessionStored" value="True" />
      <input type="hidden" name="SessionStoredError" value="" />
      <input type="hidden" name="SST" value="5838f953-c600-454c-9c47-2b3499eed21a" />
   </form>
  </body>
</html>
```

There are two version of the integrated checkout page which can be configured through the DLs below:

**Integrated v1** - DL=checkout_v1_purchase, which allows you to pre-populate the **Amount** value before launching the page.

**Integrated v2** - DL=checkout_v2_purchase, which allows your customers to enter the **Amount** and **Reference** details on the page.

**Session Response**

The response contains a secure session token (SST).

##2. Payment Page and Transaction Processing

Great, you’re onto the next part of the sequence.

```shell
cURL "https://demo.bambora.co.nz/access/index.aspx"
-d 'SessionId=[SessionId]'
-d 'SST=[Secure session token]'
```

Once the session is established, your backend application notifies your website which then POSTs (GET also supported) the Session ID and Secure Session Token to Bambora’s server URL - [https://demo.bambora.co.nz/access/index.aspx](https://demo.bambora.co.nz/access/index.aspx). You can perform this redirection in an iFrame or just redirect the entire site.

Once the payment is submitted you should expect two responses back from Bambora.

***1st response***


```html
<html>
  <body>
    <form>
      <input type="hidden" name="SessionId" value="WIZ201702241044133171a4c36a760e" />
      <input type="hidden" name="SST" value="d98eedec-6fe4-4ee4-84e3-9c90892389a5" />
      <input type="hidden" name="SessionKey" value="WIZKEY2bacb4ebda" />
      <input type="hidden" name="CustRef" value="your_custref" />
      <input type="hidden" name="CustNumber" value="your_custnumber" />
      <input type="hidden" name="Amount" value="1000" />
      <input type="hidden" name="Surcharge" value="" />
      <input type="hidden" name="AmountIncludingSurcharge" value="1000" />
      <input type="hidden" name="Result" value="1" />
      <input type="hidden" name="DeclinedCode" value="" />
      <input type="hidden" name="DeclinedMessage" value="" />
      <input type="hidden" name="Receipt" value="90892516" />
      <input type="hidden" name="TxDateTime" value="2017-02-24+13%3a10%3a15" />
      <input type="hidden" name="SettlementDate" value="2017-02-24" />
      <input type="hidden" name="MaskedCard" value="424242******4242" />
      <input type="hidden" name="CardHolderName" value="cardholder name" />
      <input type="hidden" name="ExpiryDate" value="08/19" />
      <input type="hidden" name="CardType" value="Visa" />
    </form>
  </body>
</html>
```

The Bambora server POSTs the response to your server using the ‘ServerURL’ as supplied by you in the session initiation. The following table lists the response values that Bambora will POST to your server. The response data should be stored in your database and made accessible to your website.


***2nd response***

Bambora redirect to the ‘UserURL’ as supplied in the Session Initiation request and POST’s the SessionID and SST. Your UserURL page can use these details to access the transaction results stored in your database from the first notification.

Once your UserURL page receives the transaction result, it can display the details to the customer.

And you’re done.

```html
<html>
  <body>
    <form>
      <input type="hidden" name="SessionStored" value="True" />
      <input type="hidden" name="SST" value="5838f953-c600-454c-9c47-2b3499eed21a" />
   </form>
  </body>
</html>
```
##Technical details

**Session Initiation Request - Body Schema**

Parameter |  Format | Mandatory/Optional |  Description
--------- | ------- | ----------- | -------------
Username | Alpha/Num(16) | Mandatory | API Username
Password | Alpha/Num(16) | Mandatory | API Password
Amount | Numeric(10) | Mandatory | Amount entered in cent value e.g. $55.00 = 5500
SessionID | Alpha/Num(64) | Mandatory | Merchant’s unique session identifier. Maintain this session identifier throughout the life of the transaction.
SessionKey | Alpha/Num(512) | Mandatory | Generated by the merchant, can be any value e.g. random generated number, timestamp etc. The session key is to be included for security and authentication purposes. This Key should only be passed to Bambora’s server and should not be sent to your front end. Bambora will return the SessionKey to your ServerURL in the transaction response once the payment has been processed. If you do not receive a SessionKey or receive an invalid SessionKey, then this should be treated as a possible fraudulent transaction. Bambora needs to be notified of this event along with your internal teams for investigation.
ServerURL | Alpha/Num(512) | Mandatory | This value must be Base 64 encoded. The URL of your server that Bambora will POST the first notification to including the transaction result data. If you do not require data to be sent to the ServerURL, please leave this field empty.
UserURL	| Alpha/Num(512)	| Optional	| This value must be Base 64 encoded. The URL to force the user browser to POST to; this is the second notification.
DL | Alpha/Num(16) | Mandatory | This value is used to specify the styling and functionality Bambora should display to your customer. Please find the below DLs to initate corresponding payment page. <br/></br/> checkout_v1_purchase <br/>checkout_v2_purchase
AccountNumber | Alpha/Num(16) | Mandatory | This value dictates which account the transaction will be processed through. If this value is not populated, the transaction will be processed to the account tied to the username field.
CustRef | Alpha/Num(64) | Mandatory | A reference for the transaction sent by you for reporting purposes.
CustNumber | Alpha/Num(64) | Mandatory | An additional reference for the transaction sent by you for reporting purposes.
Email | Alpha/Num(64) | Optional | An additional reference for the transaction sent by you for reporting purposes. Please note Bambora can enable this field to send an email receipt to the email address entered in this field. Please contact Bambora if you require this service.
Reference1 | Text(32) | Optional | This field name can be changed to something more suitable to your company for example “AccountID”. Please advise if you wish to change it. An additional reference that can be sent by you for reporting purposes e.g. a customer identifier unique to your system. This will be available to view and download with the transaction details in our reporting tool, Backoffice.
Reference2 | Text(64) |  Optional  | This field name can be changed to something more suitable to your company for example “AccountID”. Please advise if you wish to change it. An additional reference that can be sent by you for reporting purposes e.g. a customer identifier unique to your system. This will be available to view and download with the transaction details in our reporting tool, Backoffice.
Reference3 | Text(128) | Optional | This field name can be changed to something more suitable to your company for example “AccountID”. Please advise if you wish to change it. An additional reference that can be sent by you for reporting purposes e.g. a customer identifier unique to your system. This will be available to view and download with the transaction details in our reporting tool, Backoffice.
Reference4 | Text(1024) | Optional | This field name can be changed to something more suitable to your company for example “AccountID”. Please advise if you wish to change it. An additional reference that can be sent by you for reporting purposes e.g. a customer identifier unique to your system. This will be available to view and download with the transaction details in our reporting tool, Backoffice.
Reference5 | Text(1024) | Optional | This field name can be changed to something more suitable to your company for example “AccountID”. Please advise if you wish to change it. An additional reference that can be sent by you for reporting purposes e.g. a customer identifier unique to your system. This will be available to view and download with the transaction details in our reporting tool, Backoffice.
UserDeclinedURL | Alpha/Num(512) | Optional | Bambora offer the ability to have a cancel button on your payment page. By default this page will redirect to the UserURL. If you wish this button to redirect to a different page other than the UserURL, you can specify the UserDeclinedURL in this field. If your customer selects the cancel button on the payment page, the page will be redirected to the declined URL. Please note if you send a value in this field, all responses for declined transactions will be sent to this URL including cancelled transactions and transactions which have been declined by the bank. Please see section Cancel Button for further information.

**Session Initiation Response - Body Schema**

Parameter |  Format | Description
--------- | ------- | -----------
SessionStored | Alpha/Num(64) | Value that can be either True or False
SessionStoredError | Alpha/Num(256) | Textual description of the error. This field will only be populated if the SessionStored value is False.
SST | Alpha/Num(64) | Secure Session Token.  This field will only be populated if the SessionStored value is True.

**Payment Page Request- Body Schema**

The following table lists the values that your application forces the customer’s browser to POST/GET to Bambora.

Parameter | Format  | Description
--------- | ------- | -----------
SessionId | Alpha/Num(64) | Merchant’s unique session identifier. Defined by the merchant in the session initiation request.
SST | Alpha/Num(64) | Secure Session Token.  This field will only be populated if the SessionStored value is True.

**1st Payment Page Response - Body Schema**

The following table lists the response values that Bambora will POST to your server.

Parameter |  Format | Description
--------- | ------- | -----------
Result | Numeric(1) | 0 = Declined <br/>1 = Approve <br/> 2 = In progress <br/> 3 = Session Expired
Receipt | Numeric(8) | The Bambora receipt number generated for this transaction.
DeclinedCode | Alpha/Num(5) | Declined code interprets the error message.
DeclinedMessage | Alpha/Num(256) | Reason message for declined.
SST | Alpha/Num(64) | The Secure Session Token.
SessionKey | Alpha/Num(64)	| The session key submitted by you only to Bambora in the session initiation request.
SessionId | Alpha/Num(64) | Merchant unique session identifier.
MaskedCard | Alpha/Num(16)	| The masked credit card number. This will show first 6 and last 4 digits of card number. E.g. 123456*****4321.
ExpiryDate | Alpha/Num(5)	| The credit card expiry date. Format: MM/YY
CardHolderName | Alpha/Num(64)	| The credit card expiry date. Format: MM/YY
CardType | Alpha/Num(16) |	E.g. MasterCard, Visa.
CardSubType | Alpha/Num(64)	| A value can be passed back giving further details on the card. This field is only returned if you have the CardSubType functionality enabled.
TxDateTime | Alpha/Num(19)	| The transaction date/time. Format: YYYY-MM-DD HH:MM:SS E.g. 2018-01-20 18:32:30.
CustNumber | Alpha/Num(64)	| An additional reference for the transaction sent by you for reporting purposes.
CustRef | Alpha/Num(64) | Merchant Reference for the transaction.
Amount | Numeric(10)	| Transaction amount in cent value e.g. $55.00 = 5500. This is the originally submitted amount and does not include any surcharge amount that may have been applied by Bambora.
Surcharge | Numeric(10) | Surcharge amount applied to the transaction in cent value. This is only populated if surcharging is enabled on your account.
Totalamount | Numeric(10) |	Transaction amount plus surcharge amount applied to the transaction in cent value. This is only populated if surcharging is enabled on your account.
SettlementDate | Alpha/Num(64) |	The settlement date of the transaction returned in the following format YYYY-MM-DD.
Reference1 | Alpha/Num(32)	| Additional optional reference submitted by you in the transaction request.
Reference2 | Alpha/Num(64)	| Additional optional reference submitted by you in the transaction request.
Reference3 | Alpha/Num(128)	| Additional optional reference submitted by you in the transaction request.
Reference4 | Alpha/Num(256)	| Additional optional reference submitted by you in the transaction request.
Reference5 | Alpha/Num(1024)	| Additional optional reference submitted by you in the transaction request.

**2nd Payment Page Response - Body Schema**

The following table lists the values that Bambora will POST to your server when redirecting to your UserURL.

Parameter |   Format | Description
--------- | -------  | -----------
SessionId | Alpha/Num(64) | Merchant’s unique session identifier. Defined by the merchant in the session initiation request.
SST | Alpha/Num(64) | The Secure Session Token. Provided by Bambora in the session initiation response.

#Standalone

Checkout Standalone is a low touch integration, providing an secure hosted payment page through a separate URL. Transactions are processed in real-time and the response displayed back to the customer. There is not redirect back to your business but the transaction result can be viewed through your Bambora Backoffice reporting facility.  

##Standalone v1

If you wish to pre-populate the **Amount** on the page, you can use our Standalone v1 to capture your customers' card details.

Sample URL

[https://demo.bambora.co.nz/access/index.aspx?a=85569861&dl=Checkout_v1_hpp_purchase&accountnumber=Exa-5149&amount=1000&custref=&custref=devportal](https://demo.bambora.co.nz/access/index.aspx?a=85569861&dl=Checkout_v1_hpp_purchase&accountnumber=Exa-5149&amount=1000&custref=&custref=devportal)

```shell
cURL "https://demo.bambora.co.nz/access/index.aspx"
-d 'a=85569861'
-d 'accountnumber=[AccountNumber]'
-d 'amount=[AmountValue]'
-d 'dl=Checkout_v1_hpp_purchase'
```

##Standalone v2

If you require your customers to enter the **Amount** and **Reference** on the page, you can use our standalone checkout v2.

Sample URL

[https://demo.bambora.co.nz/access/index.aspx?a=85569861&dl=Checkout_v2_hpp_purchase&accountnumber=Exa-5149&amount=1000](https://demo.bambora.co.nz/access/index.aspx?a=85569861&dl=Checkout_v2_hpp_purchase&accountnumber=Exa-5149&amount=1000)

```shell
cURL "https://demo.bambora.co.nz/access/index.aspx"
-d 'a=85569861'
-d 'accountnumber=[AccountNumber]'
-d 'dl=Checkout_v2_hpp_purchase'
```


#Test payments using checkout

To make a payment using the checkout interface following details need to be provide in the test payment:

* Test Card Number
* Any three-digit CVC code
* Any expiration date in the future
* Reference (Mandatory if displayed on checkout page)
* Amount (Mandatory if displayed on checkout page)
