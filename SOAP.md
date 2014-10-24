money2020
=========
# SOAP API

## Endpoints

https://w1.mercurydev.net/ws/ws.asmx

## Merchant Info

MID: 003503902913105

PWD: xyz

## Using Mercury’s Web Services Libraries

Web Services are Web APIs that can be accessed over a network and/or the internet, and are executed on a remote system hosting the requested services. Mercury’s Web Service development URL is https://w1.mercurydev.net/ws/ws.asmx with the full WSDL accessed at: https://w1.mercurydev.net/ws/ws.asmx?WSDL

Any development environment supporting Web Services can add a reference to that URL and have access to the methods and classes it provides.

## CreditTransaction

Send Credit, U.S. PIN Debit, EBT, Check Authorization, CardLookup, BatchSummary and BatchClose.

Two arguments:  CreditTransaction(trans as string, password as string)

1. trans is an XML string
2. password is the string provided in the merchant information above.

## GiftTransaction

Send a gift card transactions

Two arguments:  GiftTransaction(trans as string, password as string)

1. trans is an XML string
2. password is the string provided in the merchant information above.

## Example

### Copy the following into a text file named:  soap.txt

```
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:mer="http://www.mercurypay.com">
   <soapenv:Header/>
   <soapenv:Body>
      <mer:CreditTransaction>
         <!--Optional:-->
         <mer:tran><![CDATA[<?xml version="1.0"?>
<TStream>
<Admin>
<MerchantID>111111187942001=MISMIS</MerchantID>
<TranType>PayPal</TranType>
<TranCode>OpenLocation</TranCode>
<Memo>Test mobile Wallet</Memo>
</Admin>
</TStream>]]></mer:tran>
         <mer:pw>xyz</mer:pw>
      </mer:CreditTransaction>
   </soapenv:Body>
</soapenv:Envelope>

```

### execute this command:

```
curl -k -v -X POST -H "Content-Type:text/xml;" -H 'SOAPAction:"http://www.mercurypay.com/CreditTransaction"' -d "@soap.txt" -o output.txt https://w1.mercurycert.net/ws/ws.asmx
```

### Expected result:

```
<?xml version="1.0" encoding="utf-8"?><soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"><soap:Body><CreditTransactionResponse xmlns="http://www.mercurypay.com"><CreditTransactionResult>&lt;?xml version="1.0"?&gt;
&lt;RStream&gt;
	&lt;CmdResponse&gt;
		&lt;ResponseOrigin&gt;Server&lt;/ResponseOrigin&gt;
		&lt;DSIXReturnCode&gt;000000&lt;/DSIXReturnCode&gt;
		&lt;CmdStatus&gt;Success&lt;/CmdStatus&gt;
		&lt;TextResponse&gt;Opened&lt;/TextResponse&gt;
		&lt;UserTraceData&gt;&lt;/UserTraceData&gt;
	&lt;/CmdResponse&gt;
	&lt;MerchantID&gt;111111187942001&lt;/MerchantID&gt;
&lt;/RStream&gt;
</CreditTransactionResult></CreditTransactionResponse></soap:Body></soap:Envelope>

```