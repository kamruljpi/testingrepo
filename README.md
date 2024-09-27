
# Webhook Documentation

## Introduction

A Webhook is a notification mechanism that allows you to receive information from specific events taking place in various kinds of transactions or operations such as when funds are credited to an account.

Compared to running cron  jobs or polling an endpoint, webhooks are a more scalable alternative as it is a push notification service which is less system intensive. 

Important:

Despite the convenience that it affords, implementation designs with complete dependency on webhook notifications should be avoided. Just like with any other service, 100% perpetual uptime cannot be expected.

Contingencies and backup exception handling should be put in place in the event that the webhook service is down or unavailable due to various reasons. These measures may include utilizing 1. the available [Transaction History](/docs#operation/Account-As-A-Service_RetrieveBankAccountTransactionHistory) or [Transaction Detail](/docs#operation/Account-As-A-Service_RetrieveBankAccountDetails) endpoints to confirm the status of a disbursement or collection and 2. the Transaction Page of the Netbank Virtual Partner Dashboard.

## Configuration

In order to receive webhooks, a dedicated endpoint needs to be set up on your end as a receiver or listener that can accept HTTP POST requests. This endpoint needs to be provided to Netbank to be registered in the service for you to begin receiving notifications.

Your endpoint should expect to receive content-type **text/plain;charset=ISO-8859-1**

IP whitelisting is the security mechanism that Netbank employs on its webhook service. Notifications are sent from the following IP addresses:

- **UAT: 3.0.84.126**
- **PROD: 52.74.254.158**

## Retry Mechanism

The retry mechanism is a way of ensuring that webhook events are delivered to the destination server in case of failures. The webhook service will retry if we are not able to establish any HTTP connection to your endpoint.

In case of non-200 responses, the webhook service will account for this response and will not retry. 

Retries will follow the timing below:

1st retry: 1 min

2nd retry: 4 min

3rd retry: 16 min

4th retry: 64 min

5th retry: 256 min (~4h)

## Events Available

Currently, we can support to provide webhooks for the following events:

### 1. Incoming Fund Transfer from Instapay and Pesonet

- Event name: *PROCESS\_PESONET\_INCOMING\_FUND\_TRANSFER* and *SETTLE\_INCOMING\_INSTAPAY\_TRANSFER* 
- Timing: whenever a fund transfer is credited to one of your accounts from Instapay or Pesonet
- Use Case: Local Collect and Local Collect-via-QR

Data structure:

**operationId (number)**: the unique transaction id from Netbank’s Core Banking System

**referenceNumber (string)**: the unique transaction id from the payment rails

**channel (string)**: the payment rail that was used for the fund transfer (can be PESONet, Instapay, Direct)

**externalTransferStatus (string)**: the status of the inbound transaction (can be SETTLED, PENDING, REJECTED, FOR\_SETTLEMENT)

**amount (number)**: the amount of funds that were credited to the VCA and the Netbank bank account.

**registrationTime (string)**: the date and time the funds were credited to the Netbank bank account.

**productBranchCode (string)**: the Netbank Branch Code where the Netbank bank account resides.

**recipientAccountNumber (string)**: the VCA number that received the transaction.

**recipientAccountNumberBankFormat (string)**: the Netbank bank account number where the funds were credited.

**sender.accountNumber (string)**: the account number of the source bank account that end-customers/partners used to transfer the funds

**sender.accountName (string)**: the account name of the source bank account that end-customers/partners used to transfer the funds

**sender.institutionCode (string)**: the bank code of the bank that the end-customers/partners used to transfer funds

**alias (string)**: the fixed 5-digit code that we is associated to the Netbank Bank Account where the funds were credited to

**referenceCode (string)**: the 7-digit code that you generated and concatenated with the Partner Alias to complete the Virtual Collection Account Number

**remarks (string)**: additional details passed by the payment rails

**transfertype (string)**: identifies the type of transfer, only used for QRPH transactions

Sample request:

```
{
	"operationId": 996261,
	"referenceNumber": "20220531CUOBPHM2XXXB000000000016695",
	"channel": "INSTAPAY",
	"externalTransferStatus": "SETTLED",
	"amount": 10,
	"registrationTime": "2022-05-31T11:47:58.202",
	"productBranchCode": 39,
	"recipientAccountNumber": 900010031122,
	"recipientAccountNumberBankFormat": "039-000-00005-2",
	"sender": {
		"accountNumber": 055000000014,
		"name": "Netbank",
		"institutionCode": "CUOBPHM2XXX"
	},
	"alias": 90001,
	"referenceCode"": 0031122,
	"remarks": "InstaPay transfer #20220531CUOBPHM2XXXB000000000016695",
	"transferType": "QR_P2M"
}
```

### 2. Incoming Fund Transfer from Other Netbank Accounts via the "Direct" Settlement Rail

- Event name: *ACCOUNT\_TRANSFER\_TO\_PRODUCT* 
- Timing: whenever a fund transfer from other Netbank accounts via the DIRECT settlement rail is credited to one of your accounts
- Use Case: Accounts-as-a-Service, Local Collect, Local Collect-via-QR

Data structure:

**alias (string):** the fixed 5-digit code used for Virtual Collection Accounts that is associated to the Netbank Bank Account where the funds were credited to; may return null if not applicable

**amount (number):** the amount of funds that were transferred

**channel (string):** the payment rail that was used for the fund transfer: INTRABANK

**commandId (long):** the internal system id assigned by Netbank’s core banking system to the payment operation

**operationId (number):** the unique transaction id from Netbank’s Core Banking System that was assigned to the payment

**recipient.accountName (string):** the account name of the receiving bank account where the funds have been credited to

**recipient.accountNumber (string):** the account number of the receiving bank account where the funds have been credited to

**recipient.productId (string):** the internal system id assigned by Netbank’s core banking system to the receiving bank account where the funds have been credited to

**recipientAccountNumberBankFormat (string):** the Netbank bank account number where the funds were credited to

**referenceCode (string):** the 7-digit code that you generated and concatenated with the Partner Alias to complete the Virtual Collection Account Number; may return null if not applicable

**registrationTime (string):** the date and time the funds were credited to the Netbank bank account

**Remarks (string):** additional details included by the sender

**Sender.accountName (string):** the account name of the sending bank account where the funds have originated

**Sender.accountNumber (string):** he account number of the sending bank account where the funds have originated

**Sender.productId (string):** the internal system id assigned by Netbank’s core banking system to the sending bank account where the funds have originated

**targetProductBranchCode (string):** the Netbank Branch Code where the receiving Netbank bank account resides

Sample response:
```
{
  "alias": null,
  "amount": 990,
  "channel": "INTRABANK",
  "commandId": 3789448,
  "operationId": 7306681,
  "productBranchCode": null,
  "recipient": {
    "accountNumber": "055000000014",
    "name": "Netbank",
    "institutionCode": "CUOBPHM2XXX"
  },
  "accountName": "Test",
  "accountNumber": "294-000-00021-4",
  "productId": 17325,
  "recipientAccountNumberBankFormat": "085-000-00002-5",
  "referenceCode": null,
  "registrationTime": "2024-08-08T13:30:49.10041",
  "Remarks": "test",
  "Sender": {
    "accountNumber": "055000000015",
    "name": "Netbank",
    "institutionCode": "CUOBPHM2XXX"
  },
  "targetProductBranchCode": 85
}
```

### 3. Outgoing Fund Transfer via Instapay and Pesonet

- Event name: `TRANSFER_STATUS_CHANGE`
- Timing: sent whenever a disbursement sent through Instapay or Pesonet changes its settlement status

	- INSTAPAY: 1 webhook is sent to update the status to either SETTLED or REJECTED
	
	- PESONET: 1 webhook is sent to update the status to FOR_SETTLEMENT; 1 webhook is sent again thereafter to update the status to either SETTLED or REJECTED
	
- Use Case: Local Payout and Local Payout-via-QR

#### INSTAPAY - SETTLED
Data Structure:

**amount (number):** the amount of funds that were transferred

**channel (string):** the payment rail that was used for the fund transfer (INSTAPAY)

**operationId (number):** the unique transaction id from Netbank’s Core Banking System (the “transaction_id” returned by the response of https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer)

**ownerCifNumber (string):** the sender's Customer Information File number within Netbank’s system

**productBranchCode (string):** the Netbank Branch Code where the Netbank bank account resides
purpose: API user’s own unique reference code (the value "reference_id" passed to [https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer](https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer)) + universally unique identifier (UUID) generated by Netbank’s core banking system

**recipientAccountNumber (string):** the destination bank account number of the transfer

**referenceNumber (string):** the unique transaction id from the payment rails

**registrationTime (string):** the time and date when the settlement status has changed.

**remarks (string):** API user’s own unique reference code (the value "reference_id" passed to [https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer](https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer)) + universally unique indenfitier (UUID) generated by Netbank’s core banking system

**sender.accountNumber (string):** the account number of the Netbank source bank account

**status (string):** the current settlement status of the fund transfer (SETTLED)

**statuscode (string):** the statuscode of the transfer (*if the statuscode returned is ACWP which means Accepted Without Posting, this would mean that the transfer has been received by the destination bank but have yet to be credited or rejected to the respective destination account)

**transfertype (string):** the type of transfer of the transaction (OUTGOING)

**uuid(string):** nul

Sample request:
```
{
	"amount": 888.66,
	"channel": "INSTAPAY",
	"operationId": 7177230,
	"ownerCifNumber": 13946,
	"productBranchCode": 85,
	"purpose": "TEST-020724-H_922bfcd5-4ee8-43bc-9f33-b2e53a0f5767",
	"recipientAccountNumber": "085000000025",
	"referenceNumber": "20240702CUOBPHM2XXXB000000000020818",
	"registrationTime": "2024-07-02T11:46:37.008016",
	"remarks": "TEST-020724-H_922bfcd5-4ee8-43bc-9f33-b2e53a0f5767",
	"senderAccountNumber": "085-000-00001-4",
	"status": "SETTLED",
	"statusCode": "ACTC",
	"transferType": "OUTGOING",
	"uuid": null
}
```

#### INSTAPAY - REJECTED:
Data Structure:

**amount (number):** the amount of funds that were transferred

**channel (string):** the payment rail that was used for the fund transfer (INSTAPAY)

**operationId (number):** the unique transaction id from Netbank’s Core Banking System (the “transaction_id” returned by the response of [https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer](https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer))

**ownerCifNumber (string):** the sender's Customer Information File number within Netbank’s system

**productBranchCode (string):** the Netbank Branch Code where the Netbank bank account resides

**purpose(string):** API user’s own unique reference code (the value "reference_id" passed to
[https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer](https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer)) + universally unique indenfitier (UUID) generated by Netbank’s core banking system

**recipientAccountNumber (string):** the destination bank account number of the transfer

**referenceNumber (string):** the unique transaction id from the payment rails

**registrationTime (string):** the time and date when the settlement status has changed.

**remarks (string):** the rejection error code and its corresponding meaning returned by the receiving financial institution

**sender.accountNumber (string):** the account number of the Netbank source bank account

**status (string):** the current settlement status of the fund transfer (REJECTED)

**statuscode (string):** the rejection error code returned by the receiving financial 

**institution transfertype (string):** the type of transfer of the transaction (OUTGOING)

**uuid(string):** null

Sample response:

```
{
	"amount": 441.11,
	"channel": "INSTAPAY",
	"operationId": 7183713,
	"ownerCifNumber": 13946,
	"productBranchCode": 85,
	"purpose": "TEST-040724-2R_043b5410-a24b-49b8-9da4-2de0a24da94c",
	"recipientAccountNumber": 08502400022,
	"referenceNumber": "20240704CUOBPHM2XXXB000000000021098",
	"registrationTime": "2024-07-04T13:20:43.098",
	"remarks": "AC01 (Incorrect account number)",
	"senderAccountNumber": "085-000-00001-4",
	"status": "REJECTED",
	"statusCode": "AC01",
	"transferType": "OUTGOING",
	"uuid": null
}
```

#### PESONET - FOR SETTLEMENT:
Data Structure:

**amount (number):** the amount of funds that were transferred

**channel (string):** the payment rail that was used for the fund transfer (PESONET)

**operationId (number):** the unique transaction id from Netbank’s Core Banking System (the “transaction_id” returned by the response of https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer)

**ownerCifNumber (string):** the sender's Customer Information File number within Netbank’s system

**productBranchCode (string):** the Netbank Branch Code where the Netbank bank account resides

**purpose (string):** API user’s own unique reference code (the value "reference_id" passed to https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer) + universally unique indenfitier (UUID) generated by Netbank’s core banking system

**recipientAccountNumber (string):** the destination bank account number of the transfer

**referenceNumber (string):** the unique transaction id from the payment rails

**registrationTime (string):** the time and date when the settlement status has changed.

**remarks (string):**API user’s own unique reference code (the value "reference_id" passed to https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer) + universally unique indenfitier (UUID) generated by Netbank’s core banking system

**sender.accountNumber (string):** the account number of the Netbank source bank account

**status (string):** the current settlement status of the fund transfer (FOR_SETTLEMENT)

**statuscode (string):** null

**transfertype (string):** the type of transfer of the transaction (OUTGOING)

**uuid (string):** universally unique indenfitier (UUID) assigned by the operator of PESONET

Sample response:
```
{
	"amount": 222,
	"channel": "PESONET",
	"operationId": 7177236,
	"ownerCifNumber": 13946,
	"productBranchCode": 85,
	"purpose": "TEST-020724-P1_3a3f425f-85cf-4a99-8839-70a8cac5c75a",
	"recipientAccountNumber": 085000000025,
	"referenceNumber": 2024184000000049,
	"registrationTime": "2024-07-02T12:50:00.837609",
	"remarks": "TEST-020724-P1_3a3f425f-85cf-4a99-8839-70a8cac5c75a",
	"senderAccountNumber": "085-000-00001-4",
	"status": "FOR_SETTLEMENT",
	"statusCode": null,
	"transferType": "OUTGOING",
	"uuid": "fef608c05ad14410bfc9ed9efa93eec6"
}
```

#### PESONET - SETTLED:
Data Structure:

**transfertype (string):** the type of transfer of the transaction (OUTGOING)

**status (string):** the current settlement status of the fund transfer (SETTLED)

**registrationTime (string):** the time and date when the settlement status has changed

**channel (string):** the payment rail that was used for the fund transfer (PESONET)amount (number): the amount of funds that were transferred

**referenceNumber (string):** the unique transaction id from the payment rails

**operationId (number):** the unique transaction id from Netbank’s Core Banking System (the “transaction_id” returned by the response of [https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer](https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer))

**sender.accountNumber (string):** the account number of the Netbank source bank account

**recipientAccountNumber (string):** the destination bank account number of the transfer

**ownerCifNumber (string):** the sender's Customer Information File number within Netbank’s system

**productBranchCode (string):** the Netbank Branch Code where the Netbank bank account resides

**uuid:** universally unique indenfitier (UUID) assigned by the operator of PESONET

**remarks (string):** API user’s own unique reference code (the value "reference_id" passed to [https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer](https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer)) + universally unique indenfitier (UUID) generated by Netbank’s core banking system

**purpose (string):** API user’s own unique reference code (the value "reference_id" passed to [https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer](https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer)) + universally unique identifier (UUID) generated by Netbank’s core banking system

**statuscode (string):** null

Sample response:
```
{
	"transferType": "OUTGOING",
	"status": "SETTLED",
	"registrationTime": "2024-08-16T17:42:11",
	"channel": "PESONET",
	"amount": 2,
	"referenceNumber": 2024229000001213,
	"operationId": 52077592,
	"senderAccountNumber": "029-001-00001-2",
	"recipientAccountNumber": 100590319065,
	"ownerCifNumber": 13120,
	"productBranchCode": 29,
	"Uuid": "5ec392368d6748268c63dd3d546e9579",
	"Remarks": "01A11_ce127e46-0fa5-40ff-bcc9-cc505ec672fc",
	"Purpose": "01A11_ce127e46-0fa5-40ff-bcc9-cc505ec672fc",
	"statusCode": null
}
```

#### PESONET - REJECTED:
Data Structure:

**transfertype (string):** the type of transfer of the transaction (OUTGOING)

**status (string):** the current settlement status of the fund transfer (REJECTED)

**registrationTime (string):** the time and date when the settlement status has changed

**channel (string):** the payment rail that was used for the fund transfer (PESONET)

**amount (number):** the amount of funds that were transferred

**referenceNumber (string):** the unique transaction id from the payment railsoperationId (number): the unique transaction id from Netbank’s Core Banking System (the “transaction_id” returned by the response of [https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer](https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer))

**sender.accountNumber (string):** the account number of the Netbank source bank account

**recipientAccountNumber (string):** the destination bank account number of the transfer

**ownerCifNumber (string):** the sender's Customer Information File number within Netbank’s system

**productBranchCode (string):** the Netbank Branch Code where the Netbank bank account resides

**uuid:** universally unique indenfitier (UUID) assigned by the operator of PESONET

**remarks (string):** the rejection error code and its corresponding meaning returned by the receiving financial institution

**Purpose (string):** API user’s own unique reference code (the value "reference_id" passed to [https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer](https://virtual.netbank.ph/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer)) + universally unique indenfitier (UUID) generated by Netbank’s core banking system

**statuscode (string):** the rejection error code returned by the receiving financial institution

Sample response:
```
{
	"transferType": "OUTGOING",
	"Status": "REJECTED",
	"registrationTime": "2024-08-12T17:21:57",
	"channel": "PESONET",
	"amount": 2,
	"referenceNumber": 2024225000000773,
	"operationId": 51264536,
	"senderAccountNumber": "029-001-00001-2",
	"recipientAccountNumber": 10059,
	"ownerCifNumber": 13120,
	"productBranchCode": 29,
	"Uuid": "663713f9a6db4a488210b4373b0bee76",
	"remarks": "Beneficiary account number is invalid for any reason (i.e. incorrect account number, dormant, closed, blocked, frozen",
	"Purpose": "01A11_9f8fc3ee-31aa-4f8d-b0fd-790eee5a978a",
	"statusCode": "AC03",
}
```

### 4. ECPay Credit Notification

- Event name: `PROCESS_DIRECT_INCOMING_FUND_TRANSFER`
- Timing: whenever a fund transfer is credited to one of your accounts from an `ECPAY` deposit
- Use Case: Local Collect

Data Structure:

**alias (long):** the fixed 5-digit code that we is associated to the Netbank Bank Account where the funds were credited to

**amount (BigDecimal):** the amount of funds that were credited to the VCA and the Netbank bank account

**channel (string):** the payment rail that was used for the fund transfer (“ECPAY”)

**commandId (long):** the event identifier from Netbank’s Core Banking System

**externalTransferStatus (string):** the status of the inbound transaction (“PROCESSED”)

**operationId (long):** the unique transaction id from Netbank’s Core Banking System

**productBranchCode (long):** the Netbank Branch Code where the Netbank bank account resides

**recipientAccountName (string):** the account name from the input by the depositor

**recipientAccountNumber (string):** the VCA number that received the transaction

**recipientAccountNumberBankFormat (string):** the Netbank bank account number where the funds were credited

**referenceCode (string):** the 7-digit code that you generated and concatenated with the Partner Alias to complete the Virtual Collection Account Number

**referenceNumber (string):** the unique transaction id from the payment rails

**registrationTime (localdatetime):** the date and time the funds were credited to the Netbank bank account

**remarks (string):** additional details passed by the payment rails

Sample request:

```
{
	"alias": 99997,
	"amount": 1600.33,
	"channel": "ECPAY",
	"commandId": 2209159,
	"externalTransferStatus": "PROCESSED",
	"operationId": 4370333,
	"productBranchCode": 85,
	"recipientAccountName": "ABC Company",
	"recipientAccountNumber": 999971234567,
	"recipientAccountNumberBankFormat": "085-000-00001-4",
	"referenceCode": 1234567,
	"referenceNumber": "ECPAY24082023-2",
	"registrationTime": "2023-08-24T11:13:09.373",
	"remarks": "deposit for payment"
}
```

### 5. Incoming Fund Transfer from other Netbank accounts via the Direct settlement rail

- Event name: `ACCOUNT\_TRANSFER\_TO\_PRODUCT`
- Timing: whenever a fund transfer from other Netbank accounts via the DIRECT settlement rail is credited to one of your accounts
- Use Case: Accounts-as-a-Service, Local Collect, Local Collect-via-QR

Data Structure:

**alias (string):** the fixed 5-digit code used for Virtual Collection Accounts that is associated to the Netbank Bank Account where the funds were credited to; may return null if not applicable

**amount (number):** the amount of funds that were transferred

**channel (string):** the payment rail that was used for the fund transfer: INTRABANK

**commandId (long):** the internal system id assigned by Netbank’s core banking system to the payment operation

**operationId (number):** the unique transaction id from Netbank’s Core Banking System that was assigned to the payment

**recipient.accountName (string):** the account name of the receiving bank account where the funds have been credited to

**recipient.accountNumber (string):** the account number of the receiving bank account where the funds have been credited to

**recipient.productId (string):** the internal system id assigned by Netbank’s core banking system to the receiving bank account where the funds have been credited to

**recipientAccountNumberBankFormat (string):** the Netbank bank account number where the funds were credited to

**referenceCode (string):** the 7-digit code that you generated and concatenated with the Partner Alias to complete the Virtual Collection Account Number; may return null if not applicable

**registrationTime (string):** the date and time the funds were credited to the Netbank bank account

**Remarks (string):** additional details included by the sender

**Sender.accountName (string):** the account name of the sending bank account where the funds have originated

**Sender.accountNumber (string):** he account number of the sending bank account where the funds have originated

**Sender.productId (string):** the internal system id assigned by Netbank’s core banking system to the sending bank account where thefunds have originated

**targetProductBranchCode (string):** the Netbank Branch Code where the receiving Netbank bank account resides

Sample request:
```
{
  "alias": null,
  "amount": 990,
  "channel": "INTRABANK",
  "commandId": 3789448,
  "operationId": 7306681,
  "productBranchCode": null,
  "recipient": {
    "accountNumber": "055000000014",
    "name": "Netbank",
    "institutionCode": "CUOBPHM2XXX"
  },
  "accountName": "Test",
  "accountNumber": "294-000-00021-4",
  "productId": 17325,
  "recipientAccountNumberBankFormat": "085-000-00002-5",
  "referenceCode": null,
  "registrationTime": "2024-08-08T13:30:49.10041",
  "Remarks": "test",
  "Sender": {
    "accountNumber": "055000000014",
    "name": "Netbank",
    "institutionCode": "CUOBPHM2XXX"
  },
  "targetProductBranchCode": 85
}
```

### 6. Repayments Received To Loan Accounts

- Event name: `PAY\_LOAN`
- Timing: whenever a payment is posted to a loan account
- Use Case: Loan-as-a-Service

Data Structure:

**amount (number):** the payment amount that was posted

**channel (string):** the payment channel where the payment was received

**checkno (number):** the check number if the payment was made via check

**commandid (number):** the internal system id assigned by Netbank’s core banking system to the payment operation

**currentAmortizationDueDate (string):** the amortization due date to which the payment was applied

**customerEffectiveName (string):** the customer name of the loan account holder

**customerFirstName (string):** the first name of the loan account holder

**customerLastName (string):** the last name of the loan account holder

**loanAccountNumber (string):** the account number of the loan to which the payment was posted

**loanId (string):** the internal system id assigned by Netbank’s core banking system to the loan account

**nextDueDate (string):** the subsequent amortization due date of the loan account

**operationId (number):** the unique transaction id from Netbank’s Core Banking System that was assigned to the payment

**outstandingAmortizations (string):** the number of remaining amortizations for the loan account

**paymentStatus (string):** the status of the loan payment that was received

**Paymenttype (string):** indicates how the payment was posted to the loan account

**productBranchCode (string):** the Netbank Branch Code where the loan account resides

**sourceAccount (string):** the account number of the source bank account that end-customers/partners used to transfer the funds

**sourceAccountBranchCode (string):** the Netbank Branch Code where the sender account number resides

**sourceAccountSenderName (string):** the account name of the source bank account that end-customers/partners used to ransfer the funds

**transactionDate (string):** the time and date when the payment was posted

Sample request:
```
{
  "amount": 3583.21,
  "channel": "GENERAL LEDGER",
  "checkNo": null,
  "commandId": 3787114,
  "currentAmortizationDueDate": "2024-08-25",
  "customerEffectiveName": "Morgan, Dexter",
  "customerFirstName": "Dexter",
  "customerLastName": "Morgan",
  "loanAccountNumber": "085-29-00057-7",
  "loanId": 83295,
  "nextDueDate": "2024-09-10",
  "operationId": 7303141,
  "outstandingAmortizations": 2,
  "outstandingBalance": 6666.44,
  "paymentStatus": "PROCESSED",
  "paymentType": "MEMO",
  "productBranchCode": 85,
  "sourceAccount": null,
  "sourceAccountBranchCode": null,
  "sourceAccountSenderName": null,
  "transactionDate": "2024-08-07T10:25:31.405"
}
```

# Virtual Collection Accounts (VCA)

## Overview

Virtual Collection Accounts (VCA) is one of Netbank’s Banking-as-a-Service solutions that enables you, our Partners, to accept fully- traceable bank transfer payments from clients. 

Through this solution, you are able to **freely generate and assign reference numbers that act as virtual accounts** that you can provide to your payors. These virtual accounts act as beneficiary account numbers for payors to transfer funds to.


The funds credited to virtual accounts are settled in your Netbank Corporate Account, enabling easier reconciliation.

To illustrate, let’s suppose you have a Netbank Corporate Bank Account Number which is 123-111-99999-1.

Instead of providing that account number to your payors, you can assign a Virtual Collection Account to a payor as a destination account when they make payments via fund transfer. For the purposes of this illustration, let’s say that the VCA assigned to customer A is “999971234567”.

When the payment to VCA “999971234567” is made, funds are credited to your Netbank Corporate Bank Account Number 123-111- 99999-1. Since both the assignment and generation of the VCA was done on your end, you should now be able to reconcile what the payment is for or who the payment was from.

There are generally no (or very minimal) APIs needed to integrate VCAs, making it a very flexible plug-and-play solution for Partners looking to implement a collection solution.

You also can receive notifications directly from our system to yours via webhooks as soon as we receive the inbound transaction.

## Generating Virtual Collection Accounts

Generating Virtual Collection Accounts do not require any API integration. Instead, it is a standard format of generating the accounts that simply need to be followed. VCA numbers are generally 12-digit numbers that are comprised of:

**5-digit Partner Alias + 7-digit Reference Number**

**Netbank will assign the 5-digit Partner Alias** while the rest of the VCA number can freely be assigned by you or your system.

To enable Netbank to assign your Partner Alias, you will need to provide Netbank your nominated Netbank Corporate Account that will receive the payments credited to VCAs.


## Using Virtual Collection Accounts

Virtual Collection Accounts are highly-flexible.

You can use VCAs by assigning them on a per-invoice basis where your payors will remit to different VCAs per transaction.

On the flipside, you can use VCAs by assigning them similar to a dedicated bank account where your users will remit to the same VCA for all of their transactions.

However you decide, Netbank does not need to be notified on your preferred use-case. 


## Features and Benefits of VCA’s

The VCA being a collection solution, allows for easier payment reconciliation. As you have visibility of the VCA that your payors are crediting, once funds are received, tagging payables as paid is much easier. This also eliminates the extra step for payors to send a proof of payment.

For fintechs and businesses looking to roll out a collection solution that is readily available to as many payors as possible, VCAs are recognized by the rails of Instapay and Pesonet. This means that payors, using their own banking or e-wallet apps, can immediately transfer funds to VCAs. There is no need to integrate separately with popular platforms like GCash, Maya, Unionbank or BPI which can be tedious, costly and time-consuming.

As VCAs are essentially a masking of your Netbank Corporate account, whenever a fund transfer occurs, they are settled directly with Netbank. The settlement schedule follows solely that of Instapay and Pesonet, eliminating clearing delays that are commonly experienced with other payment aggregators where there is normally further batching and crediting.


## Making Payments to VCA’s

The user journey of paying a VCA is simple and straightforward: users log on to their favorite banking or e-wallet apps (GCash, Maya, Unionbank, BDO, BPI, etc)

1. Select the option where can perform a fund transfer (normally labelled “Transfer” or “Send Money to Other Banks”),

1. Look for “Netbank” in the list of banks,

1. Enter the amount, the account name of your Netbank Corporate Account and the **VCA as the destination account number**.

![](/images/making_payment.jpeg)


## Payment Confirmation

There are several ways to confirm if a payment into a VCA has been received, allowing for great flexibility for system and operational integration for your apps/platforms or workflow.

1. Via the Channel Transactions page of the [Netbank Virtual Partner Dashboard](/home), you can view funds you have received in the UAT and PRODUCTION environment under the Collect Tab. Transaction details are displayed in an easy-to-understand and straightforward tabular format that includes the destination VCA that the funds were paid to. 

	![](/images/Payment_Confirmation.jpeg)

1. We can register an endpoint that you will expose in order to receive credit notification webhooks. You may refer to the documentation on webhooks [here ](/docs#section/Webhook-Documentation)for further information.

1. Via our [Retrieve Bank Account Transaction History](/docs#operation/Account-As-A-Service_RetrieveBankAccountTransactionHistory) API endpoint, you can use a VCA number in the path parameter to obtain a list of transactions. Kindly refer to our API documentation for further details.


## QRPH Integration

Generating QRPH images for Virtual Collection Accounts is also possible. Kindly refer to our API documentation [here](/docs#operation/QR-PH_GenerateQRPH).

You can use a specific Virtual Collection Account as the “destination\_account” in the Generate QRPH Image endpoint. This way, payors would not have to manually type in their target VCA when performing a fund transfer but instead, upload or scan a QR image. This minimizes or even eliminates the possibility of payors transferring to erroneous account details.


## Testing and Integration

There is little to no API integration that would need to be done to implement Virtual Collection Accounts to your system.

In order to commence testing, you may contact your Client Success Manager via [support@netbank.ph ](mailto:support@netbank.ph)or an alternative communication point that they have established for you to receive assistance during integration. Your client Success Managers would ask you for

1. The UAT bank account account (that you have opened via the Netbank Virtual Partner Dashboard) where your assigned 5-digit Partner Alias would be associated and 

1. A URL endpoint that you have exposed where we can push credit notification webhooks.

To simulate incoming transfers in the UAT environment, kindly utilize Netbank’s [Process Account-To-Account Fund Transfer](/docs#operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer) API. You can open and nominate another UAT bank account and designate it as your source account.

Feel free to reach out to your Client Success Managers for assistance on testing and other questions.




