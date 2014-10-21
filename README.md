money2020
=========
Accessing the MPS REST based API

1.  copy the following into a text file named:  json.txt

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

2. execute this command:

curl -k -v -X POST -H "Content-Type:application/json" -H "Authorization: Basic NDk0OTAxOnh5eg==" -d "@json.txt" -o output.txt https://w1.mercurycert.net/PaymentsAPI/Credit/Sale

3. Expected result:

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

# Note the api url will change depending on the transaction type (e.g. /Credit/Return will send a Credit Return txn).
