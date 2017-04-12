# Handling with API
Transaction operations can be performed either using the Bambora Checkout or by using the API. The following will describe handling transactions with API.

----

This section describes how to handle transactions using the Transaction-API. Please refer to the [API-reference](/checkout/apis/transaction) for the full specification. 

1. Purchase: Collect the money from the users bank account.
2. Capture: Collect the money from the users bank account after an pre-authorization has been made. 

## Submit a single transaction

<p><span class="badge">POST</span><span class="fg-primary text-sm">https://demo.bambora.co.nz/interface/api/dts.asmx</span></p>

The body below provides an overview of the available transaction elements that should be submitted in the XML request.

```json
{
      <CustNumber>your_custnumber</CustNumber>
      <CustRef>your_custref</CustRef>
      <Amount>1000</Amount>
      <TrnType>1</TrnType>
      <AccountNumber>your_accountnumber</AccountNumber>
      <CreditCard>
                  <CardNumber>4242424242424242</CardNumber>
                  <ExpM>02</ExpM>
                  <ExpY>2019</ExpY>
                  <CVN>123</CVN>
                  <CardHolderName>CardHolderName</CardHolderName>
       </CreditCard>
       <Security>
                  <UserName>your_api_username</UserName>
                  <Password>your_api_password</Password>
       </Security>
}
```

## Capture a transaction

<p><span class="badge">POST</span><span class="fg-primary text-sm">https://demo.bambora.co.nz/interface/api/dts.asmx</span></p>

The body below provides an overview of the available transaction elements that should be submitted in the XML request.

```json
{
<Capture>
         <Receipt>12345678</Receipt> 
         <Amount>5500</Amount> 
         <Security> 
                 <UserName>your_api_username</UserName> 
                 <Password>your_api_password</Password>
         </Security> 
</Capture>
}
```
