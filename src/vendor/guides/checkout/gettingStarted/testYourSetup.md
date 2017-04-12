# Test Your Setup

When you have completed your integration it is important to test your setup. If you want to perform a quick test Bambora Checkout offers an easy way to perform a test transaction. When the payment window is shown press `Ctrl + q` to shown to available test cards. Click the desired card to fill the input fields. This menu can also be invoked an a mobile device by shaking the device.

## Available test cards

Use the cards in the table below to test different card type in your application. You can use different cards to generate corresponding transaction responses.

### Approved Response

The following cards can be used to generate approved test transactions. To simulate Approved transactions, the amount entered must be over $4.00.

<table class="table"><thead>
<tr>
<th>Card Type</th>
<th>Card Number</th>
</tr>
</thead><tbody>
<tr><td>VISA</td><td>4005 5500 0000 0001 </td></tr>
<tr><td>MasterCard</td><td>5123 4567 8901 2346 </td></tr>
</tbody></table>

### Approved Response - with Timeout

Card Number  | Response | Daily Details | Example
-------------| ---------| ------------- | --------
5123 4561 2345 6787 | Approved | Simulates a time-out/delay.<br/><br/>If used in API testing, the delay happens after the transaction is submitted to the Bambora transaction switch (DTS). This can be used to simulate a time-out where the client is using query to find the status of a transaction, or where a client is submitting a void/reversal in the case of a time-out.<br/><br/>The cents value sent will dictate how many seconds delay within the API response to simulate. | To simulate a 35 second delay, you can use 10.35 or 1.35 etc.. (dollar value can be anything)<br/>To simulate a 90 second delay, you can use 10.90 etc.

### Declined Response

To simulate decline response as per the below table where a specific card number simulates a specific response code, enter any value for the amount, expiry date,CVN and cardholder name. i.e entering Visa card number 4123456789010145 with any amount will generate a decline response of 14.

All Decline codes range from 1 to 999. A lot of these are reserved for future use and so do not appear here.Please obtain the latest ‘Bank and Bambora Response Codes’ document for more detail on the responses.

This list does not contain all the responses –only those that are most common.

Card Type |Card Number |Response Code |Response Text
-----| ------------- | ------------- | ----------
Visa | 4123456789010012 | 1 | Refer Card Issuer
Visa | 4123456789010020 | 2 | Special Conditions
Visa | 4123456789010038 | 3 |Invalid Merchant
Visa | 4123456789010046 | 4 | Pick Up Card
Visa | 4123456789010053 | 5 | Do Not Honour
Visa | 4123456789010061 | 6 | Error
Visa | 4123456789010079 | 7 | Pick Up Card Special Condition
Visa | 4123456789010095 | 9 | Request In Progress
Visa | 4123456789010129 | 12 | Invalid Transaction
Visa | 4123456789010137 | 13 | Invalid Amount
Visa | 4123456789010145 | 14 | Invalid Card Number
Visa | 4123456789010152 | 15 | No Such Issuer
Visa | 4123456789010178 | 17 | Customer Cancellation
Visa | 4123456789010186 | 18 | Customer Dispute
Visa | 4123456789010194 | 19 | Re-Enter Transaction
Visa | 4123456789010202 | 20 | Invalid Response
Visa | 4123456789010210 | 21 | No Action Taken
Visa | 4123456789010228 | 22 | Suspected Malfunction
Visa | 4123456789010236 | 23 | Unacceptable Transaction Fee
Visa | 4123456789010244 | 24 | File Update Not Supported By Receiver
Visa | 4123456789010251 | 25 | Unable To Locate Record On File
Visa | 4123456789010269 | 26 | Duplicate Update Record, Old Record Replaced
Visa | 4123456789010277 | 27 | File Update Field Edit Error
Visa | 4123456789010285 | 28 | File Update File Locked Out
Visa | 4123456789010293 | 29 | File Update Not Successful, Contact Acquirer
Visa | 4123456789010301 | 30 | Invalid Pin Block
Visa | 4123456789010319 | 31 | Acquirer Not Supported By Switch
Visa | 4123456789010327 | 32 | Completed Partially
Visa | 4123456789010335 | 33 | Invalid Expiry Date
Visa | 4123456789010343 | 34 | Suspected Fraud
Visa | 4123456789010350 | 35 | Card Acceptor Contact Acquirer
Visa | 4123456789010368 | 36 | Restricted Card
Visa | 4123456789010376 | 37 | Contact Security – Acquirer
Visa | 4123456789010384 | 38 | Pin Tries Exceeded – Card Retained
Visa | 4123456789010392 | 39 | No Credit Account
Visa | 4123456789010400 | 40 | Request Function Not Supported
Visa | 4123456789010418 | 41 | Lost Card
Visa | 4123456789010426 | 42 | No Universal Account/Closed Account
Visa | 4123456789010434 | 43 | Stolen Card
Visa | 4123456789010442 | 44 | No Investment Account
Visa | 4123456789010517 | 51 | Insufficient Funds
Visa | 4123456789010525 | 52 | No Account Of Type Requested
Visa | 4123456789010533 | 53 | No Savings Account
Visa | 4123456789010541 | 54 | Invalid Expiry Date
Visa | 4123456789010558 | 55 | Invalid Pin
Visa | 4123456789010566 | 56 | No Card Record/Card Not Found
Visa | 4123456789010574 | 57 | Transaction Not Permitted To Cardholder
Visa | 4123456789010582 | 58 | Transaction Not Permitted At Terminal
Visa | 4123456789010590 | 59 | Suspected Fraud
Visa | 4123456789010608 | 60 | Contact Terminal Acquirer
Visa | 4123456789010616 | 61 | Amount Limit Exceeded
Visa | 4123456789010624 | 62 | Restricted Card
Visa | 4123456789010632 | 63 | Security Violation
Visa | 4123456789010640 | 64 | Original Amount Incorrect
Visa | 4123456789010657 | 65 | Exceeds Withdrawal Frequency Limit
Visa | 4123456789010665 | 66 | Contact Security Acquirer
Visa | 4123456789010673 | 67 | Suspected Counterfeit Card
Visa | 4123456789010681 | 68 | Transaction Timed Out
Visa | 4123456789010756 | 75 | Excessive Pin Tries
Visa | 4123456789010905 | 90 | Settlement/Cutover In Process
Visa | 4123456789010913 | 91 | Card Issuer Unavailable
Visa | 4123456789010921 | 92 | Transaction Cannot Be Completed
Visa | 4123456789010939 | 93 | Duplicate Transmission
Visa | 4123456789010947 | 94 | Reconciliation, Cutover Or Checkpoint Error
Visa | 4123456789010954 | 95 | System/Component Malfunction
Visa | 4123456789010962 | 96 | System Malfunction
Visa | 4123456789010970 | 97 | MAC Incorrect
Visa | 4123456789010988 | 98 | No Communications Key Available For Use
Visa | 4123456789010996 | 99 | Unknown Error
Visa | 4123456789011077 | 107 | Invalid transaction Amount
Visa | 4123456789011101 | 110 | Invalid credit card number
Visa | 4123456789011119 | 111 | Invalid credit card expiry date
Visa | 4123456789015003 | 500 | Batch Record Exception
Visa | 4123456789015011 | 501 | IP Address Maximum Amount for Period
Visa | 4123456789015029 | 502 | IP Address Maximum Count for Period
Visa | 4123456789015037 | 503 | CustRef Maximum Amount for Single Payment
Visa | 4123456789015045 | 504 | CustRef Maximum Amount For Period
Visa | 4123456789015052 | 505 | CustRef Maximum Count For Period
Visa | 4123456789015060 | 506 | CustNumber Maximum Amount for Single Payment
Visa | 4123456789015078 | 507 | CustNumber Maximum Amount For Period
Visa | 4123456789015086 | 508 | CustNumber Maximum Count For Period
Visa | 4123456789015094 | 509 | User Defined Value Maximum Amount for Single Payment
Visa | 4123456789015102 | 510 | User Defined Value Maximum Amount For Period
Visa | 4123456789015110 | 511 | User Defined Value Maximum Count For Period
Visa | 4123456789015128 | 512 | Single Credit Card Maximum Amount for Single Payment
Visa | 4123456789015136 | 513 | Single Credit Card Maximum Amount for Period
Visa | 4123456789015144 | 514 | Single Credit Card Maximum Count for Period
Visa | 4123456789015151 | 515 | Credit Card Number List
Visa | 4123456789015169 | 516 | Credit Card BIN Range
Visa | 4123456789015987 | 598 | Insufficient details to perform risk management check
Visa | 4123456789015995 | 599 | Exception when processing risk management check
Visa | 4123456789016001 | 600 | CC Manually Refunded
Visa | 4123456789017009 | 700 | Invalid Disbursement XML
Visa | 4123456789017017 | 701 | Disbursement XML amount total does not match the transaction amount
Visa | 4123456789017025 | 702 | Account number must be supplied for each disbursement
Visa | 4123456789017033 | 703 | Account number supplied for disbursement does not exist or account is inactive
Visa | 4123456789017041 | 704 | Account number supplied for disbursement appears two or more times
Visa | 4123456789018007 | 800 | Cannot find matching transaction to VOID
Visa | 4123456789018015 | 801 | Unable to contact acquirer
Visa | 4123456789018023 | 802 | VOID Declined by acquirer
Visa | 4123456789018031 | 803 | VOID after settlement
Visa | 4123456789018049| 804| Transaction not approved
Visa | 4123456789018056| 805| Transaction already voided
Visa | 4123456789019963| 996| Transaction is pending approval
Visa | 4123456789019971| 997| Remote Interface Exception
Visa | 4123456789019989| 998| Transaction Payment Cancelled
Visa | 4123456789019997| 999| Timeout when waiting for a response
MasterCard | 5123456789010019| 1 | Refer Card Issuer
MasterCard | 5123456789010027| 2 | Special Conditions
MasterCard | 5123456789010035| 3 | Invalid Merchant
MasterCard | 5123456789010043| 4| Pick Up Card
MasterCard | 5123456789010050 | 5 |Do Not Honour

