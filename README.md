# Withdrawals-API

## 1. Withdrawal Request
Call this endpoint to initiate a withdrawal
```
https://kbx.kubitx.com/prow/x/transfer/withdraw
```
### Sample Request URL
```
https://kbx.kubitx.com/prow/x/transfer/withdraw?asset_code=GHCX&dest=1234567890&dest_extra=058&memo=Joe Olu&branch_code
```
### Request Parameters
Name|Description
----|-----------
asset_code|Asset code of token to withdraw 
dest|Account number/MoMo
dest_extra|Destination bank code 
memo|Name on the account number/MoMo 
branch_code|Bank branch code only for GHCX asset[optional]

### Sample Response
A successful withdrawal request will return the following JSON encoded response

**HTTP 200 OK**
```javascript
{
    "account_id": "GCDPFG7HMUFPMLVKBGZGLHZH4MYSPMT5TXZJHAYHS7XRQTHXZEQ4CPLI",
    "memo_type": "text",
    "min_amount": "500",
    "max_amount": 20000.0,
    "memo": "1733T4612284251",
    "fee_percent": 0.01
}
```
Response Fields|Description
----|----------------------
account_id|Stellar public key address
memo_type|Stellar memo type id, text, hash
min_amount|Minimum amount to withdraw
max_amount|Maximum amount to withdraw
memo|Stellar memo
fee_percent|Percentage Fee that will be charged for the withdrawal

## 1.1 Endpoint to fetch List of Banks
```
https://kbx.kubitx.com/prow/x/transfer/banks
```

### Sample Response
A successful withdrawal request will return the following JSON encoded response

**HTTP 200 OK**
```javascript
{
    "account_id": "GCDPFG7HMUFPMLVKBGZGLHZH4MYSPMT5TXZJHAYHS7XRQTHXZEQ4CPLI",
    "memo_type": "text",
    "min_amount": "500",
    "max_amount": 20000.0,
    "memo": "1733T4612284251",
    "fee_percent": 0.01
}
```

Response Fields|Description
----|----------------------
banks|List of Banks


## 1.2 Endpoint to fetch Branch Codes for GHCX Withdrawals
```
https://kbx.kubitx.com/prow/x/transfer/bank-branch/{bankId}
```
### Sample Request URL
```
https://kbx.kubitx.com/prow/x/transfer/bank-branch/10123
```
### Request Parameters
Name|Description
----|-----------
bankId|bankId for a specific bank from the request made to retrieve banks in 1.1 above

# Sample Response
A successful request will return the following JSON encoded response

**HTTP 200 OK**
```javascript
{
    "account_id": "GCDPFG7HMUFPMLVKBGZGLHZH4MYSPMT5TXZJHAYHS7XRQTHXZEQ4CPLI",
    "memo_type": "text",
    "min_amount": "500",
    "max_amount": 20000.0,
    "memo": "1733T4612284251",
    "fee_percent": 0.01
}
```

Response Fields|Description
----|----------------------
branch_codes|List of Branch Codes for the selected Bank





# 2. Send Funds to account_id from 1
To complete the Withdrawal Request, send the Gross Amount [Amount to be withdrawn + fees] in the asset code being withdrawn to the account_id returned by https://kbx.kubitx.com/prow/x/transfer/withdraw from #1 using the memo returned in the response


## 2.1 Withdrawal Completion
Once the payment from 2 above is received, the withdrawal request will be processed and funds transferred to the destination Bank or MoMo account



## Response Error Codes
A failed request for any of the APIs will result in one of the following HTTP response error codes.

HTTP Code|HTTP Status|Description
---------|-----------|------------
400|Bad Request|one or more query parameters is incorrect
404|Not Found|Asset code not available
500|Server Error|the server encountered an error
