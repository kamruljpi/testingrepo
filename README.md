Webhook Documentation

Introduction

A Webhook is a notification mechanism that allows you to receive information from specific events taking place in various kinds of transactions or operations such as when funds are credited to an account.

` `Compared to running cron  jobs or polling an endpoint, webhooks are a more scalable alternative as it is a push notification service which is less system intensive. 

Important:![](Aspose.Words.4d901b5c-c735-4b23-9164-17a21d8af389.001.png)

Despite the convenience that it affords, implementation designs with complete dependency on webhook notifications should be avoided. Just like with any other service, 100% perpetual uptime cannot be expected.

Contingencies and backup exception handling should be put in place in the event that the webhook service is down or unavailable due to various reasons. These measures may include utilizing 1. the available [Transaction History](https://virtual.netbank.ph/docs#tag/Account-As-A-Service/operation/Account-As-A-Service_RetrieveBankAccountTransactionHistory) or [Transaction Detail](https://virtual.netbank.ph/docs#tag/Account-As-A-Service/operation/Account-As-A-Service_RetrieveBankAccountDetails) endpoints to confirm the status of a disbursement or collection and 2. the Transaction Page of the Netbank Virtual Partner Dashboard.

Configuration

In order to receive webhooks, a dedicated endpoint needs to be set up on your end as a receiver or listener that can accept HTTP POST requests. This endpoint needs to be provided to Netbank to be registered in the service for you to begin receiving notifications.

Your endpoint should expect to receive content-type **text/plain;charset=ISO-8859-1**

IP whitelisting is the security mechanism that Netbank employs on its webhook service. Notifications are sent from the following IP addresses:

- **UAT: 3.0.84.126**
- **PROD: 52.74.254.158**

Retry Mechanism

The retry mechanism is a way of ensuring that webhook events are delivered to the destination server in case of failures. The webhook service will retry if we are not able to establish any HTTP connection to your endpoint.

In case of non-200 responses, the webhook service will account for this response and will not retry. Retries will follow the timing below:

1st retry: 1 min

2nd retry: 4 min

3rd retry: 16 min

4th retry: 64 min

5th retry: 256 min (~4h)

Events Available

Currently, we can support to provide webhooks for the following events:

Incoming Fund Transfer from Instapay and Pesonet![](Aspose.Words.4d901b5c-c735-4b23-9164-17a21d8af389.002.png)

Event name: *PROCESS\_PESONET\_INCOMING\_FUND\_TRANSFER* and *SETTLE\_INCOMING\_INSTAPAY\_TRANSFER* Timing: whenever a fund transfer is credited to one of your accounts from Instapay or Pesonet

Use Case: Local Collect and Local Collect-via-QR

Data structure:

***operationId (number)**: the unique transaction id from Netbank’s Core Banking System*

***referenceNumber (string)**: the unique transaction id from the payment rails*

***channel (string)**: the payment rail that was used for the fund transfer (can be PESONet, Instapay, Direct)*

***externalTransferStatus (string)**: the status of the inbound transaction (can be SETTLED, PENDING, REJECTED, FOR\_SETTLEMENT)*

***amount (number)**: the amount of funds that were credited to the VCA and the Netbank bank account.*

***registrationTime (string)**: the date and time the funds were credited to the Netbank bank account.*

***productBranchCode (string)**: the Netbank Branch Code where the Netbank bank account resides.*

***recipientAccountNumber (string)**: the VCA number that received the transaction.*

***recipientAccountNumberBankFormat (string)**: the Netbank bank account number where the funds were credited. **sender.accountNumber (string)**: the account number of the source bank account that end-customers/partners used to transfer the funds.*

***sender.accountName (string)**: the account name of the source bank account that end-customers/partners used to transfer the funds. **sender.institutionCode (string)**: the bank code of the bank that the end-customers/partners used to transfer funds.*

***alias (string)**: the fixed 5-digit code that we is associated to the Netbank Bank Account where the funds were credited to. **referenceCode (string)**: the 7-digit code that you generated and concatenated with the Partner Alias to complete the Virtual Collection Account Number*

***remarks (string)**: additional details passed by the payment rails.*

***transfertype (string)**: identifies the type of transfer, only used for QRPH transactions* Sample request:

1  {
1  "operationId": 996261,
1  "referenceNumber": "20220531CUOBPHM2XXXB000000000016695",
1  "channel": "INSTAPAY",
1  "externalTransferStatus": "SETTLED",
1  "amount": 10,
1  "registrationTime": "2022-05-31T11:47:58.202",
1  "productBranchCode": "39",
1  "recipientAccountNumber": "900010031122",
1  "recipientAccountNumberBankFormat": "039-000-00005-2",
1  "sender": {
1  "accountNumber": "055000000014",

<table><tr><th colspan="1" rowspan="2"></th><th colspan="1" valign="top"><p>13	 "name": "Netbank",</p><p>14	 "institutionCode": "CUOBPHM2XXX”</p><p>15	 },</p><p>16	 "alias": “90001”,</p><p>17	 "referenceCode": "0031122",</p><p>18	 "remarks": "InstaPay transfer #20220531CUOBPHM2XXXB000000000016695”</p><p>19	 "transferType": QR_P2M</p><p>20	 }</p></th><th colspan="2" rowspan="2"></th></tr>
<tr><td colspan="1"></td></tr>
<tr><td colspan="1" rowspan="2"></td><td colspan="1" valign="top"><p>Outgoing Fund Transfer via Instapay and Pesonet</p><p>Event name: <i>TRANSFER_STATUS_CHANGE</i></p><p>Timing: sent whenever a disbursement sent through Instapay or Pesonet changes its settlement status Use Case: Local Payout and Local Payout-via-QR</p><p>Data Structure:</p><p><i><b>amount (number)</b>: the amount of funds that were transferred</i></p><p><i><b>channel (string)</b>: he payment rail that was used for the fund transfer (can be PESONet, Instapay)</i></p><p><i><b>operationId (number)</b>: the unique transaction id from Netbank’s Core Banking System</i> </p><p><i><b>ownerCifNumber (string)</b>: the sender's Customer Information File number within Netbank’s system</i></p><p><i><b>productBranchCode (string)</b>: the Netbank Branch Code where the Netbank bank account resides</i></p><p><i><b>recipientAccountNumber (string)</b>: the destination bank account number of the transfer</i></p><p><i><b>referenceNumber (string)</b>: the unique transaction id from the payment rails</i></p><p><i><b>registrationTime (string)</b>: the time and date when the settlement status has changed.</i></p><p><i><b>sender.accountNumber (string)</b>: the account number of the Netbank source bank account</i></p><p><i><b>status (string)</b>: the current settlement status of the fund transfer (SETTLED or REJECTED for INSTAPAY; FOR_SETTLEMENT, SETTLED or REJECTED for PESONET)</i></p><p><i><b>transfertype (string)</b>: the type of transfer of the transaction (OUTGOING)</i></p><p>Sample request:</p></td><td colspan="1" rowspan="2"></td></tr>
<tr><td colspan="1" valign="top"><p>1	 {</p><p>&emsp;2	 "amount": 222.44,</p><p>&emsp;3	 "channel": "INSTAPAY",</p><p>&emsp;4	 "operationId": 3883673,</p><p>&emsp;5	 "ownerCifNumber": 13946,</p><p>&emsp;6	 "productBranchCode": 85,</p><p>&emsp;7	 "recipientAccountNumber": "085000000025",</p><p>&emsp;8	 "referenceNumber": "20230711CUOBPHM2XXXB000000000022515",</p><p>&emsp;9	 "registrationTime": "2023-07-11T15:24:59.648752",</p><p>11	 "senderAccountNumber": "085-000-00001-4",</p><p>12	 "status": "SETTLED",</p><p>13	 "transferType": "OUTGOING"</p><p>14	 }</p></td></tr>
<tr><td colspan="1"></td><td colspan="1" valign="top"><p>![](Aspose.Words.4d901b5c-c735-4b23-9164-17a21d8af389.003.png) ECPay Credit Notification</p><p>- Event name: <i>PROCESS_DIRECT_INCOMING_FUND_TRANSFER</i></p><p>- Timing: whenever a fund transfer is credited to one of your accounts from an ECPAY deposit</p><p>- Use Case: Local Collect</p><p>Data Structure:</p><p><b><i>alias (long)</i></b>: the fixed 5-digit code that we is associated to the Netbank Bank Account where the funds were credited to <b><i>amount (BigDecimal)</i></b>: the amount of funds that were credited to the VCA and the Netbank bank account</p></td><td colspan="2"></td></tr>
</table>
***channel (string)***: the payment rail that was used for the fund transfer (“ECPAY”)![](Aspose.Words.4d901b5c-c735-4b23-9164-17a21d8af389.004.png)

***commandId (long)***: the event identifier from Netbank’s Core Banking System

***externalTransferStatus (string)***: the status of the inbound transaction (“PROCESSED”)

***operationId (long)***: the unique transaction id from Netbank’s Core Banking System

***productBranchCode (long)***: the Netbank Branch Code where the Netbank bank account resides

***recipientAccountName (string)***: the account name from the input by the depositor

***recipientAccountNumber (string)***: the VCA number that received the transaction

***recipientAccountNumberBankFormat (string)***: the Netbank bank account number where the funds were credited

***referenceCode (string)***: the 7-digit code that you generated and concatenated with the Partner Alias to complete the Virtual Collection Account Number

***referenceNumber (string)***: the unique transaction id from the payment rails

***registrationTime (localdatetime)***: the date and time the funds were credited to the Netbank bank account

***remarks (string)***: additional details passed by the payment rails

1  {
1  "alias": 99997,
1  "amount": 1600.33,
1  "channel": "ECPAY",
1  "commandId": 2209159,
1  "externalTransferStatus": "PROCESSED",
1  "operationId": 4370333,
1  "productBranchCode": 85,
1  "recipientAccountName": "ABC Company",
10  "recipientAccountNumber": 999971234567,
10  "recipientAccountNumberBankFormat": "085-000-00001-4",
10  "referenceCode": 1234567,
10  "referenceNumber": "ECPAY24082023-2",
10  "registrationTime": "2023-08-24T11:13:09.373",
10  "remarks": "deposit for payment"
10  }
