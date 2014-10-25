money2020
=========

# Mobile Wallet API

## Endpoints

SOAP Endpoint:  https://w1.mercurycert.net/ws/ws.asmx

Mobile Wallet Endpoint:  https://wh.mercurycert.net/wallet

## Merchant Info
MID: 111111187942001=MISMIS

PWD: xyz

OperatorId: Test

## Checkin Simulation
Location:  https://devtools-PayPal.com/local-simulator/login_consumer  

UserName: peep1@mercurypay.com

Password: Mercury1

Check in Merchant Lat/Long: 44, -121

Check in Merchant:  Cert Merchant1 MIS
 
## Process

1.  Customer checkes in using PayPal mobile wallet and/or generates QR code with Wells Fargo wallet.

2.  Customer is ready to pay for goods/services and requests to pay via PayPal or Wells Fargo

3.  If Wells Fargo POS Operator scans QR code and submits credit/sale txn with example below.

4.  If PayPal POS Operator retrieves a list of customers currently checked in, finds the picture of the consumer, selects the photo, and submits credit/sale txn with example below.

5.  In both cases POS keeps data returned for subsequent txns (void, return, adjust, etc.).

## Example Wells Fargo

Data in EncryptedBlock is the value parsed from the QR code read

Data in EncryptedFormat shows that this is a WellsFargo txn

### Copy the following into a text file named mw.txt

````
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:mer="http://www.mercurypay.com">
   <soapenv:Header/>
   <soapenv:Body>
      <mer:CreditTransaction>
         <mer:tran><![CDATA[<?xml version="1.0"?>
	<TStream>
          <Transaction>
          <MerchantID>111111187942001=MISMIS</MerchantID>
          <OperatorID>test</OperatorID>
          <TranType>Credit</TranType>
          <TranCode>Sale</TranCode>
          <Memo>Team1 money2020</Memo>
          <InvoiceNo>123456</InvoiceNo>
          <RefNo>123456</RefNo>
		  <RecordNo>RecordNumberRequested</RecordNo>
		  <Frequency>OneTime</Frequency>
          <Amount>
              <Purchase>2.26</Purchase>
          </Amount>
          <Account>
		<EncryptedBlock>5004273327621157534=92051717061615938</EncryptedBlock>
		<EncryptedFormat>WellsFargo</EncryptedFormat>
		<AccountSource>2dBarCode</AccountSource>
          </Account>
          </Transaction>
        </TStream>]]></mer:tran>
         <mer:pw>xyz</mer:pw>
      </mer:CreditTransaction>
   </soapenv:Body>
</soapenv:Envelope>
````

### execute this command:

```
curl -k -v -X POST -H "Content-Type:text/xml;" -H 'SOAPAction:"http://www.mercurypay.com/CreditTransaction"' -d "@mw.txt" -o output.txt https://w1.mercurycert.net/ws/ws.asmx
```

### Expected result:

````
<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"><soap:Body><VerifyPaymentResponse xmlns="http://www.mercurypay.com/"><VerifyPaymentResult><ResponseCode>0</ResponseCode><Status>Error</Status><StatusMessage>Payment ID Expired.</StatusMessage><DisplayMessage>We are unable to process the payment because this payment session has expired.</DisplayMessage><AvsResult /><CvvResult /><AuthCode /><Token /><RefNo /><Invoice>1234</Invoice><AcqRefData /><CardType /><MaskedAccount /><Amount>2.22</Amount><TaxAmount>0</TaxAmount><TransPostTime>0001-01-01T00:00:00</TransPostTime><CardholderName /><AVSAddress /><AVSZip /><TranType>Sale</TranType><PaymentIDExpired>true</PaymentIDExpired><CustomerCode /><Memo>Team1 money2020</Memo><AuthAmount>0</AuthAmount><VoiceAuthCode /><ProcessData /><OperatorID>dano</OperatorID><TerminalName /><ExpDate /></VerifyPaymentResult></VerifyPaymentResponse></soap:Body></soap:Envelope>
````

## PayPal Example

Data in EncryptedBlock is the customerId returned from the Customers request

Data in EncryptedFormat shows that this is a PayPal txn

### Get list of Customers...copy the following into a text file named mw.txt

````
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:mer="http://www.mercurypay.com">
   <soapenv:Header/>
   <soapenv:Body>
      <mer:CreditTransaction>
         <mer:tran><![CDATA[<?xml version="1.0"?>
	<TStream>
          <Admin>
          <MerchantID>111111187942001=MISMIS</MerchantID>
          <OperatorID>dano</OperatorID>
          <TranType>PayPal</TranType>
          <TranCode>Customers</TranCode>
          <Memo>Team1 money2020</Memo>
          </Admin>
        </TStream>]]></mer:tran>
         <mer:pw>xyz</mer:pw>
      </mer:CreditTransaction>
   </soapenv:Body>
</soapenv:Envelope>
````

### execute this command:

```
curl -k -v -X POST -H "Content-Type:text/xml;" -H 'SOAPAction:"http://www.mercurypay.com/CreditTransaction"' -d "@mw.txt" -o output.txt https://w1.mercurycert.net/ws/ws.asmx
```

### Expected result:

````
<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"><soap:Body><CreditTransactionResponse xmlns="http://www.mercurypay.com"><CreditTransactionResult>&lt;?xml version="1.0"?&gt;
&lt;RStream&gt;
	&lt;CmdResponse&gt;
		&lt;ResponseOrigin&gt;Server&lt;/ResponseOrigin&gt;
		&lt;DSIXReturnCode&gt;000000&lt;/DSIXReturnCode&gt;
		&lt;CmdStatus&gt;Success&lt;/CmdStatus&gt;
		&lt;TextResponse&gt;1 Customer&lt;/TextResponse&gt;
		&lt;UserTraceData&gt;&lt;/UserTraceData&gt;
	&lt;/CmdResponse&gt;
	&lt;MerchantID&gt;111111187942001&lt;/MerchantID&gt;
	&lt;OperatorID&gt;dano&lt;/OperatorID&gt;
	&lt;Customers&gt;eyJjdXN0b21lcnMiOlt7ImltYWdlVXJsIjoiaHR0cDovL2kuZWJheWltZy5jb20vMDAvcy9NekF3V0RNd01BPT0vei9xUkFBQU94eWZTcFNURktMLyRUMmVDMTZSSEpHc0ZGTXVleVdUaUJTVEZLS2dULSF+fjYwXzIuSlBHIiwiY3VzdG9tZXJJZCI6IlZVQU41VFhQWDRDWU4iLCJhcnJpdmFsRGF0ZSI6IjIwMTQtMTAtMjVUMjI6MTU6MjdaIiwiY3VzdG9tZXJOYW1lIjoiVml0byBOLiJ9XX0=&lt;/Customers&gt;
&lt;/RStream&gt;
</CreditTransactionResult></CreditTransactionResponse></soap:Body></soap:Envelope>
````

### Decode the data in the Customers element

````
{"customers":[{"imageUrl":"http://i.ebayimg.com/00/s/MzAwWDMwMA==/z/qRAAAOxyfSpSTFKL/$T2eC16RHJGsFFMueyWTiBSTFKKgT-!~~60_2.JPG","customerId":"VUAN5TXPX4CYN","arrivalDate":"2014-10-25T22:15:27Z","customerName":"Vito N."}]}
````

### Copy the following into mw.txt to execute a credit sale

Use the data in the customerId for EncryptedBlock

Use PayPal for the EncryptedFormat

````
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:mer="http://www.mercurypay.com">
   <soapenv:Header/>
   <soapenv:Body>
      <mer:CreditTransaction>
         <mer:tran><![CDATA[<?xml version="1.0"?>
	<TStream>
          <Transaction>
          <MerchantID>111111187942001=MISMIS</MerchantID>
          <OperatorID>test</OperatorID>
          <TranType>Credit</TranType>
          <TranCode>Sale</TranCode>
          <Memo>Team1 money2020</Memo>
          <InvoiceNo>123456</InvoiceNo>
          <RefNo>123456</RefNo>
		  <RecordNo>RecordNumberRequested</RecordNo>
		  <Frequency>OneTime</Frequency>
          <Amount>
              <Purchase>2.26</Purchase>
          </Amount>
          <Account>
		<EncryptedBlock>VUAN5TXPX4CYN</EncryptedBlock>
		<EncryptedFormat>PayPal</EncryptedFormat>
		<AccountSource>2dBarCode</AccountSource>
          </Account>
          </Transaction>
        </TStream>]]></mer:tran>
         <mer:pw>xyz</mer:pw>
      </mer:CreditTransaction>
   </soapenv:Body>
</soapenv:Envelope>
````

### Execute the following command

````
curl -k -v -X POST -H "Content-Type:text/xml;" -H 'SOAPAction:"http://www.mercurypay.com/CreditTransaction"' -d "@mw.txt" -o output.txt https://w1.mercurycert.net/ws/ws.asmx
````

### Expected Result

````
<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"><soap:Body><CreditTransactionResponse xmlns="http://www.mercurypay.com"><CreditTransactionResult>&lt;?xml version="1.0"?&gt;
&lt;RStream&gt;
	&lt;CmdResponse&gt;
		&lt;ResponseOrigin&gt;Processor&lt;/ResponseOrigin&gt;
		&lt;DSIXReturnCode&gt;000000&lt;/DSIXReturnCode&gt;
		&lt;CmdStatus&gt;Approved&lt;/CmdStatus&gt;
		&lt;TextResponse&gt;AP&lt;/TextResponse&gt;
		&lt;UserTraceData&gt;&lt;/UserTraceData&gt;
	&lt;/CmdResponse&gt;
	&lt;TranResponse&gt;
		&lt;MerchantID&gt;111111187942001&lt;/MerchantID&gt;
		&lt;AcctNo&gt;650600XXXXXX4646&lt;/AcctNo&gt;
		&lt;ExpDate&gt;XXXX&lt;/ExpDate&gt;
		&lt;CardType&gt;DCVR&lt;/CardType&gt;
		&lt;TranCode&gt;Sale&lt;/TranCode&gt;
		&lt;AuthCode&gt;010628&lt;/AuthCode&gt;
		&lt;CaptureStatus&gt;Captured&lt;/CaptureStatus&gt;
		&lt;RefNo&gt;1001&lt;/RefNo&gt;
		&lt;InvoiceNo&gt;123456&lt;/InvoiceNo&gt;
		&lt;OperatorID&gt;test&lt;/OperatorID&gt;
		&lt;Memo&gt;Team1 money2020&lt;/Memo&gt;
		&lt;Amount&gt;
			&lt;Purchase&gt;2.26&lt;/Purchase&gt;
			&lt;Authorize&gt;2.26&lt;/Authorize&gt;
		&lt;/Amount&gt;
		&lt;AcqRefData&gt;b000000000000628c0000e000j377772141025182817&lt;/AcqRefData&gt;
		&lt;RecordNo&gt;OyBUc7tm2++XgyyWr9d+tdbJZl8Qv+QzoDhOXEecuGoiEgUQCSIQB8xD&lt;/RecordNo&gt;
		&lt;ProcessData&gt;|00|210100200000&lt;/ProcessData&gt;
	&lt;/TranResponse&gt;
&lt;/RStream&gt;
</CreditTransactionResult></CreditTransactionResponse></soap:Body></soap:Envelope>
````

## MerchantConfig Example

### Copy this into mw.txt

````
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:mer="http://www.mercurypay.com">
   <soapenv:Header/>
   <soapenv:Body>
      <mer:CreditTransaction>
         <mer:tran><![CDATA[<?xml version="1.0"?>
	<TStream>
          <Admin>
          <MerchantID>111111187942001=MISMIS</MerchantID>
          <OperatorID>dano</OperatorID>
          <TranType>Mercury</TranType>
          <TranCode>MerchantConfig</TranCode>
          <Memo>Team1 money2020</Memo>
          </Admin>
        </TStream>]]></mer:tran>
         <mer:pw>xyz</mer:pw>
      </mer:CreditTransaction>
   </soapenv:Body>
</soapenv:Envelope>
````

### execute this command:

```
curl -k -v -X POST -H "Content-Type:text/xml;" -H 'SOAPAction:"http://www.mercurypay.com/CreditTransaction"' -d "@mw.txt" -o output.txt https://w1.mercurycert.net/ws/ws.asmx
```

### Expected result:

````
<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"><soap:Body><CreditTransactionResponse xmlns="http://www.mercurypay.com"><CreditTransactionResult>&lt;?xml version="1.0"?&gt;
&lt;RStream&gt;
	&lt;CmdResponse&gt;
		&lt;ResponseOrigin&gt;Server&lt;/ResponseOrigin&gt;
		&lt;DSIXReturnCode&gt;000000&lt;/DSIXReturnCode&gt;
		&lt;CmdStatus&gt;Success&lt;/CmdStatus&gt;
		&lt;TextResponse&gt;APPROVED&lt;/TextResponse&gt;
		&lt;UserTraceData&gt;&lt;/UserTraceData&gt;
	&lt;/CmdResponse&gt;
	&lt;MerchantID&gt;111111187942001&lt;/MerchantID&gt;
	&lt;OperatorID&gt;dano&lt;/OperatorID&gt;
	&lt;MerchantConfig&gt;eyJtb2JpbGVXYWxsZXRzIjpbeyJNb2JpbGVXYWxsZXRJbnB1dFR5cGUiOiJGT1RPIiwiTW9iaWxlV2FsbGV0VHlwZSI6IlBheVBhbCJ9LHsiTW9iaWxlV2FsbGV0SW5wdXRUeXBlIjoiUVIiLCJNb2JpbGVXYWxsZXRUeXBlIjoiV2VsbHNGYXJnbyJ9XX0=&lt;/MerchantConfig&gt;
&lt;/RStream&gt;
</CreditTransactionResult></CreditTransactionResponse></soap:Body></soap:Envelope>
````

### Decode the data in the MerchantConfig element

This shows us that the merchant is setup for both PayPal and WellsFargo wallets.  The merchant accepts PayPal PhotoCheckin and WellsFargo QR code and allows us to setup our UI dynamically to accept future mobile wallet providers and input types.

````
{"mobileWallets":[{"MobileWalletInputType":"FOTO","MobileWalletType":"PayPal"},{"MobileWalletInputType":"QR","MobileWalletType":"WellsFargo"}]}
````
