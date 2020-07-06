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
branch_code|Bank branch code only for GHCX asset [optional for withdrawals to a MoMo account or banks that have hasBranch == false]

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

---

## 1.1 Endpoint to fetch List of Banks
```
https://kbx.kubitx.com/prow/x/transfer/banks
```

### Sample Response
A successful withdrawal request will return the following JSON encoded response

**HTTP 200 OK**
```javascript
{
    "data": [
        {
            "id": 1,
            "title": "ACCESS BANK PLC",
            "code": "044",
            "currency": "NGN",
            "country": "NG",
            "hasBranch": false,
            "isMomo": false
        },
        {
            "id": 2,
            "title": "CITI BANK",
            "code": "023",
            "currency": "NGN",
            "country": "NG",
            "hasBranch": false,
            "isMomo": false
        },
        .
        .
        .
        {
            "id": 20,
            "title": "ZENITH BANK PLC",
            "code": "057",
            "currency": "NGN",
            "country": "NG",
            "hasBranch": false,
            "isMomo": false
        },
        {
            "id": 24,
            "title": "Vodafone Mobile Money",
            "code": "VODAFONE",
            "currency": "GHS",
            "country": "GH",
            "hasBranch": false,
            "isMomo": true
        },
        {
            "id": 25,
            "title": "Airtel Mobile Money",
            "code": "AIR",
            "currency": "GHS",
            "country": "GH",
            "hasBranch": false,
            "isMomo": true
        },
        {
            "id": 28,
            "title": "ECOBANK",
            "code": "GH130100",
            "currency": "GHS",
            "country": "GH",
            "hasBranch": true,
            "isMomo": false
        }
    ],
    "meta": {
        "countries": [
            {
                "code": "NG",
                "prefix": "234",
                "name": "Nigeria"
            },
            {
                "code": "GH",
                "prefix": "233",
                "name": "Ghana"
            }
        ]
    },
    "status": "success"
}
```

Response Fields|Description
----|----------------------
data|List of Banks
meta|List of countries [can be used to segment/filter the list of banks by country]
id|The bankId field [to be used to fetch the list of branches for banks that have hasBranch == true]
code|The code for the bank [to be provided as a param in the Withdraw Request]
  
---

## 1.2 Endpoint to fetch Branch Codes for GHCX Withdrawals
```
https://kbx.kubitx.com/prow/x/transfer/bank-branch/{bankId}
```
### Sample Request URL
```
https://kbx.kubitx.com/prow/x/transfer/bank-branch/28
```
### Request Parameters
Name|Description
----|-----------
bankId|bankId for a specific bank from the request made to retrieve banks in [1.1](https://github.com/kbxwallet/Withdrawals-API#endpoint-to-fetch-list-of-banks) above

### Sample Response
A successful request will return the following JSON encoded response

**HTTP 200 OK**
```javascript
{
    "data": [
        {
            "branch_code": "GH130998",
            "branch_name": "System Branch"
        },
        {
            "branch_code": "GH130101",
            "branch_name": "ECOBANK GH ACCRA MAIN"
        },
        {
            "branch_code": "GH130103",
            "branch_name": "ECOBANK GH RING ROAD"
        },
        .
        .
        .
        {
            "branch_code": "GH130155",
            "branch_name": "ECOBANK(GH)LTD- FIRST NAT SAV AND LOANS"
        },
        {
            "branch_code": "GH130159",
            "branch_name": "ECOBANK (GH) LTD-FIRST ALLIED SAV AND LO"
        }
    ],
    "status": "success"
}
```

Response Fields|Description
----|----------------------
data|List of Branch Data for the selected Bank
branch_code|The branch code for the bank; to be provided as a parameter in the [Withdraw Request](https://github.com/kbxwallet/Withdrawals-API#1-withdrawal-request)
branch_name|The branch name for the bank

---  
  
## 2. Send Funds to account_id from the [response](https://github.com/kbxwallet/Withdrawals-API#sample-response) from the Withdraw Request

To complete the Withdrawal Request, send the Gross Amount [Amount to be withdrawn + fees(fee_percent * amount)] in the asset code being withdrawn to the account_id using the memo returned in the response


## Withdrawal Completion
Once the payment from 2 above is received, the withdrawal request will be processed and funds transferred to the destination Bank or MoMo account

---

## Response Error Codes
A failed request for any of the APIs will result in one of the following HTTP response error codes.

HTTP Code|HTTP Status|Description
---------|-----------|------------
400|Bad Request|one or more query parameters is incorrect
404|Not Found|Asset code not available
500|Server Error|the server encountered an error
