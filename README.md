# Webhook Documentation

## Introduction

A Webhook is a notification mechanism that allows you to receive information from specific events taking place in various kinds of transactions or operations such as when funds are credited to an account.

Compared to running cron  jobs or polling an endpoint, webhooks are a more scalable alternative as it is a push notification service which is less system intensive. 

Important:![](Aspose.Words.4d901b5c-c735-4b23-9164-17a21d8af389.001.png)

Despite the convenience that it affords, implementation designs with complete dependency on webhook notifications should be avoided. Just like with any other service, 100% perpetual uptime cannot be expected.

Contingencies and backup exception handling should be put in place in the event that the webhook service is down or unavailable due to various reasons. These measures may include utilizing 1. the available [Transaction History](https://virtual.netbank.ph/docs#tag/Account-As-A-Service/operation/Account-As-A-Service_RetrieveBankAccountTransactionHistory) or [Transaction Detail](https://virtual.netbank.ph/docs#tag/Account-As-A-Service/operation/Account-As-A-Service_RetrieveBankAccountDetails) endpoints to confirm the status of a disbursement or collection and 2. the Transaction Page of the Netbank Virtual Partner Dashboard.

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

#### 1. Incoming Fund Transfer from Instapay and Pesonet![](Aspose.Words.4d901b5c-c735-4b23-9164-17a21d8af389.002.png)

- Event name: *PROCESS\_PESONET\_INCOMING\_FUND\_TRANSFER* and *SETTLE\_INCOMING\_INSTAPAY\_TRANSFER* 
- Timing: whenever a fund transfer is credited to one of your accounts from Instapay or Pesonet
- Use Case: Local Collect and Local Collect-via-QR

Data structure:

***operationId (number)**: the unique transaction id from Netbank’s Core Banking System*

***referenceNumber (string)**: the unique transaction id from the payment rails*

***channel (string)**: the payment rail that was used for the fund transfer (can be PESONet, Instapay, Direct)*

***externalTransferStatus (string)**: the status of the inbound transaction (can be SETTLED, PENDING, REJECTED, FOR\_SETTLEMENT)*

***amount (number)**: the amount of funds that were credited to the VCA and the Netbank bank account.*

***registrationTime (string)**: the date and time the funds were credited to the Netbank bank account.*

***productBranchCode (string)**: the Netbank Branch Code where the Netbank bank account resides.*

***recipientAccountNumber (string)**: the VCA number that received the transaction.*

***recipientAccountNumberBankFormat (string)**: the Netbank bank account number where the funds were credited.*

**sender.accountNumber (string)**: the account number of the source bank account that end-customers/partners used to transfer the funds.*

***sender.accountName (string)**: the account name of the source bank account that end-customers/partners used to transfer the funds.*

**sender.institutionCode (string)**: the bank code of the bank that the end-customers/partners used to transfer funds.*

***alias (string)**: the fixed 5-digit code that we is associated to the Netbank Bank Account where the funds were credited to.*

**referenceCode (string)**: the 7-digit code that you generated and concatenated with the Partner Alias to complete the Virtual Collection Account Number*

***remarks (string)**: additional details passed by the payment rails.*

***transfertype (string)**: identifies the type of transfer, only used for QRPH transactions*

Sample request:

```
{
	"operationId": 996261,
	"referenceNumber": "20220531CUOBPHM2XXXB000000000016695",
	"channel": "INSTAPAY",
	"externalTransferStatus": "SETTLED",
	"amount": 10,
	"registrationTime": "2022-05-31T11:47:58.202",
	"productBranchCode": "39",
	"recipientAccountNumber": "900010031122",
	"recipientAccountNumberBankFormat": "039-000-00005-2",
	"sender": {
		"accountNumber": "055000000014",
		"name": "Netbank",
		"institutionCode": "CUOBPHM2XXX”
	},
	"alias": “90001”,
	"referenceCode": "0031122",
	"remarks": "InstaPay transfer #20220531CUOBPHM2XXXB000000000016695”
	"transferType": QR_P2M
}
```

#### 2. Outgoing Fund Transfer via Instapay and Pesonet

- Event name: `TRANSFER_STATUS_CHANGE`
- Timing: sent whenever a disbursement sent through Instapay or Pesonet changes its settlement status
- Use Case: Local Payout and Local Payout-via-QR

Data Structure:

***amount (number):**  the amount of funds that were transferred*

***channel (string):** he payment rail that was used for the fund transfer (can be `PESONet, Instapay`)*

***operationId (number):** the unique transaction id from Netbank’s Core Banking System*

***ownerCifNumber (string):** the sender's Customer Information File number within Netbank’s system*

***productBranchCode (string):** the Netbank Branch Code where the Netbank bank account resides*

***recipientAccountNumber (string):** the destination bank account number of the transfer*

***referenceNumber (string):** the unique transaction id from the payment rails*

***registrationTime (string):** the time and date when the settlement status has changed.*

***sender.accountNumber (string):** the account number of the Netbank source bank account*

***status (string):** the current settlement status of the fund transfer (SETTLED or REJECTED for INSTAPAY; FOR_SETTLEMENT,
SETTLED or REJECTED for PESONET)*

***transfertype (string):** the type of transfer of the transaction (OUTGOING)*

Sample request:
```
{
	"amount": 222.44,
	"channel": "INSTAPAY",
	"operationId": 3883673,
	"ownerCifNumber": 13946,
	"productBranchCode": 85,
	"recipientAccountNumber": "085000000025",
	"referenceNumber": "20230711CUOBPHM2XXXB000000000022515",
	"registrationTime": "2023-07-11T15:24:59.648752",
	"senderAccountNumber": "085-000-00001-4",
	"status": "SETTLED",
	"transferType": "OUTGOING"
}
```

#### 3. ECPay Credit Notification

- Event name: `PROCESS_DIRECT_INCOMING_FUND_TRANSFER`
- Timing: whenever a fund transfer is credited to one of your accounts from an `ECPAY` deposit
- Use Case: Local Collect

Data Structure:

***alias (long):** the fixed 5-digit code that we is associated to the Netbank Bank Account where the funds were credited to*

***amount (BigDecimal):** the amount of funds that were credited to the VCA and the Netbank bank account*

***channel (string):** the payment rail that was used for the fund transfer (“ECPAY”)*

***commandId (long):** the event identifier from Netbank’s Core Banking System*

***externalTransferStatus (string):** the status of the inbound transaction (“PROCESSED”)*

***operationId (long):** the unique transaction id from Netbank’s Core Banking System*

***productBranchCode (long):** the Netbank Branch Code where the Netbank bank account resides*

***recipientAccountName (string):** the account name from the input by the depositor*

***recipientAccountNumber (string):** the VCA number that received the transaction*

***recipientAccountNumberBankFormat (string):** the Netbank bank account number where the funds were credited*

***referenceCode (string):** the 7-digit code that you generated and concatenated with the Partner Alias to complete the Virtual Collection Account Number*

***referenceNumber (string):** the unique transaction id from the payment rails*

***registrationTime (localdatetime):** the date and time the funds were credited to the Netbank bank account*

***remarks (string):** additional details passed by the payment rails*

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
