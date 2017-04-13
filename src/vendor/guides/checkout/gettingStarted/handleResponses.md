# Handle Responses 

This section is part of the guide describing how to integrate Bambora Checkout. To handle the responses please make sure that you have [created a payment](/checkout/guides/getting-started/create-payment) using the Intergrated Checkout. 

----
Handling responses from Bambora Checkout is important. It enables you to update your order dependent on the result of the payment and ensure a good user flow. 

Bambora Integrated Checkout provides two notification of the transaction upon the completion to you in two steps. 

1. ServerURL - The first step is a server to server post between IPP and your application providing a more robust and secure way of sending the transaction response data to you. 
2. UserURL - The second step redirects the customer back to your website where they are notified of the transaction result. 

## Server-URL

Once the payment is completed Bambora will POST the first notification that including the transaction result data to the ServerURL as supplied in the Checkout session. The response data should be stored in your database and made accessible to your website. 

This value must be Base 64 encoded when being submitted in the Checkout session. If you do not require data to be sent to the ServerURL, please leave this field empty. 

## User-URL

Bambora redirects the user to the ‘UserURL’ as supplied in the Checkout session request and POSTs the SessionID and SST. The UserURL page can use these details to access the transaction results stored in your database from the first notification. 

Once your UserURL page receives the transaction result, it can display the transaction results to the user. 

## Configure the URLs
The configuration of the different URLs is done when creating the Checkout Session. Assign an object of URLs as follow to the `serverurl`-parameter and `userurl`-parameter. 

```json
{
  "ServerURL": "https://example.org/SessionResult"
  "UserURL": "https://example.org/SessionResult"
}
```
The example provided only contains two parameters and the following presents all available parameters. 

### 1st Response POST to SeverURL 
<table class="table"><thead>
<tr>
<th>Parameter</th>
<th>Format</th>
<th>Description</th>
<th style="text-align: center">Always returned</th>
</tr>
</thead><tbody>
<tr>
<td><strong>Result</strong></td>
<td>Numberic</td>
<td>0 = Declined;1 = Approve;2 = In progress;3 = Session Expired </td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>Reciept</strong></td>
<td>Numberic</td>
<td>The Bambora receipt number generated for this transaction</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>DeclinedCode</strong></td>
<td>Alpha/Num</td>
<td>Declined code interprets the error message. If result = 0, then this will be the reason for the declined transaction as an error code. If result = 1, then this will be blank
</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>DeclinedMessage	</strong></td>
<td>Alpha/Num</td>
<td>Reason message for declined. If result = 0, then this will be the textual description of the error. If result = 1, then this will be blank</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>SST</strong></td>
<td>Alpha/Num</td>
<td>The Secure Session Token</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>SessionKey</strong></td>
<td>Alpha/Num</td>
<td>The session key submitted by you only to Bambora in the session initiation request</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>SessionId</strong></td>
<td>Alpha/Num</td>
<td>Merchant unique session identifier</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>MaskedCard</strong></td>
<td>Numeric</td>
<td>The masked credit card number. This will show first 6 and last 4 digits of card number. E.g. 1234 56** ***4 321</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>ExpiryDate</strong></td>
<td>Alpha/Num</td>
<td>The credit card expiry date. Format: MM/YY</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>CardHolderName</strong></td>
<td>Alpha/Num</td>
<td>The card holder name (if captured)</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>CardType</strong></td>
<td>Alpha/Num</td>
<td>E.g. MasterCard, Visa</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>CardSubType</strong></td>
<td>Alpha/Num</td>
<td>A value can be passed back giving further details on the card. This field is only returned if you have the CardSubType functionality enabled. Please see section Card SubType Recongition for further information</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>TxDateTime</strong></td>
<td>Alpha/Num</td>
<td>The transaction date/time. Format: YYYY-MM-DD HH:MM:SS E.g. 2017-01-20 18:32:30 </td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>CustNumber</strong></td>
<td>Alpha/Num</td>
<td>An additional referencor the transaction sent by you for reporting purposes</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>CustRef</strong></td>
<td>Alpha/Num</td>
<td>Merchant Reference for the transaction.</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>Amount</strong></td>
<td>Numeric</td>
<td>Transaction amount in cent value e.g. $55.00 = 5500. 
This is the originally submitted amount and does not include any surcharge amount that may have been applied by Bambora</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>Surcharge</strong></td>
<td>Numeric</td>
<td>Surcharge amount applied to the transaction in cent value. This is only populated if surcharging is enabled on your account</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>Totalamount</strong></td>
<td>Numeric</td>
<td>Transcation amount plus surcharge amount applied to the transaction in cent value. This is only populated if surcharging is enabled on your account</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>SettlementDate</strong></td>
<td>Alpha/Num</td>
<td>The settlement date of the transaction returned in the following format YYYY-MM-DD</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>Reference1</strong></td>
<td>Alpha/Num</td>
<td>Additional optional reference submitted by you in the transaction request</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>Reference2</strong></td>
<td>Alpha/Num</td>
<td>Additional optional reference submitted by you in the transaction request</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>Reference3</strong></td>
<td>Alpha/Num</td>
<td>Additional optional reference submitted by you in the transaction request</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>Reference4</strong></td>
<td>Alpha/Num</td>
<td>Additional optional reference submitted by you in the transaction request</td>
<td style="text-align: center">No</td>
</tr>
<tr>
<td><strong>Reference5</strong></td>
<td>Alpha/Num</td>
<td>Additional optional reference submitted by you in the transaction request</td>
<td style="text-align: center">No</td>
</tr>
</tbody></table>

### 2st Response Post to UserURL 

<table class="table"><thead>
<tr>
<th>Parameter</th>
<th>Format</th>
<th>Description</th>
<th style="text-align: center">Always returned</th>
</tr>
</thead><tbody>
<tr>
<td><strong>SessionId</strong></td>
<td>Alpha/Num</td>
<td>Merchant’s unique session identifier. Defined by the merchant in the session initiation request</td>
<td style="text-align: center">Yes</td>
</tr>
<tr>
<td><strong>SST</strong></td>
<td>Alpha/Num</td>
<td>The Secure Session Token. Provided by Bambora in the session initiation response</td>
<td style="text-align: center">Yes</td>
</tr>
</tbody></table>

Next step in the proces of integration Bambora Checkout is to [test your integration](/checkout/guides/getting-started/test-your-setup) by performing test transactions.
