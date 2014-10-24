money2020
=========

# REST API

## Endpoint

https://w1.mercurydev.net/ws/ws.asmx

## Merchant Info

MID: 003503902913105

PWD: xyz

## Authentication

Authentication occurs via HTTP Basic Auth using an HTTP authorization header.

Username: existing MerchantID Password: created and stored by Mercury

1. Create string of [username]:[password]
2. BASE64 encoded string
3. Set Authorization header value to: "Basic [encoded string]"

## Content-Type

Set Content-Type header value to: “application/json”

## POSTS

All transactions are posted to https://w1.mercurycert.net/PaymentsAPI (for development and testing) followed by the transaction type at the end of the URL (referred to in this document as the Resource URL). For example, a credit sale would be: https://w1.mercurycert.net/PaymentsAPI/Credit/Sale

## Example

### Copy the following into a text file named:  json.txt

```
{
"InvoiceNo":"1",
"RefNo":"1",
"Memo":"Team2 Money2020",
"Purchase":"1.00",
"Frequency":"OneTime",
"RecordNo":"RecordNumberRequested",
"EncryptedFormat":"MagneSafe",
"AccountSource":"Swiped",
"EncryptedBlock":"2F8248964608156B2B1745287B44CA90A349905F905514ABE3979D7957F13804705684B1C9D5641C",
"EncryptedKey":"9500030000040C200026",
"OperatorID":"money2020",
}

```

### execute this command:

```
curl -k -v -X POST -H "Content-Type:application/json" -H "Authorization: Basic NDk0OTAxOnh5eg==" -d "@json.txt" -o output.txt https://w1.mercurycert.net/PaymentsAPI/Credit/Sale
```

### Expected result:

```
{
  "ResponseOrigin": "Processor",
  "DSIXReturnCode": "000000",
  "CmdStatus": "Declined",
  "TextResponse": "DECLINE",
  "UserTraceData": "",
  "MerchantID": "494901",
  "AcctNo": "549999XXXXXX6781",
  "ExpDate": "XXXX",
  "CardType": "M/C",
  "TranCode": "Sale",
  "RefNo": "1",
  "InvoiceNo": "1",
  "OperatorID": "test",
  "Memo": "Team1 Money2020",
  "Purchase": "1.00",
  "Authorize": "1.00",
  "AcqRefData": "K",
  "RecordNo": "mlXYKwJzMlo19BDSGg6v5Mgj26Lb2ypZ00IFh9zkcMwiEgUQCSIQBxrI",
  "ProcessData": "|00|210100700000"
}
```