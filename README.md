# Withdrawals-API

# Withdrawal Request
Call this endpoint to initiate a withdrawal
```
https://kbx/kubitx.com/prow/x/transfer/withdraw
```

### Sample Request URL
```
https://kbx/kubitx.com/prow/x/transfer/withdraw?asset_code=GHCX&dest=1234567890&dest_extra=058&memo=Joe Olu&branch_code
```
## Request Parameters
Name|Description
----|-----------
asset_code|Asset code of token to withdraw 
dest|Account number/MoMo
dest_extra|Destination bank code 
memo|Name on the account number/MoMo 
branch_code|Bank branch code only for GHCX asset



# Sample Response
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
fee_percent|Percentage Fee that will charged for the withdrawal
