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

![](Aspose.Words.421236d4-1bb5-4719-9c66-44ad9f5bf7f8.001.jpeg)


## Payment Confirmation

There are several ways to confirm if a payment into a VCA has been received, allowing for great flexibility for system and operational integration for your apps/platforms or workflow.

1. Via the Channel Transactions page of the [Netbank Virtual Partner Dashboard](/home), you can view funds you have received in the UAT and PRODUCTION environment under the Collect Tab. Transaction details are displayed in an easy-to-understand and straightforward tabular format that includes the destination VCA that the funds were paid to. 

	![](Aspose.Words.421236d4-1bb5-4719-9c66-44ad9f5bf7f8.002.jpeg)

1. We can register an endpoint that you will expose in order to receive credit notification webhooks. You may refer to the documentation on webhooks [here ](https://netbank.atlassian.net/wiki/spaces/NVP/pages/104693778)for further information.

1. Via our [Retrieve Bank Account Transaction History](/docs#tag/Account-As-A-Service/operation/Account-As-A-Service_RetrieveBankAccountTransactionHistory) API endpoint, you can use a VCA number in the path parameter to obtain a list of transactions. Kindly refer to our API documentation for further details.


## QRPH Integration

Generating QRPH images for Virtual Collection Accounts is also possible. Kindly refer to our API documentation [here](/docs#tag/QR-PH/operation/QR-PH_CreateQRPH).

You can use a specific Virtual Collection Account as the “destination\_account” in the Generate QRPH Image endpoint. This way, payors would not have to manually type in their target VCA when performing a fund transfer but instead, upload or scan a QR image. This minimizes or even eliminates the possibility of payors transferring to erroneous account details.


## Testing and Integration

There is little to no API integration that would need to be done to implement Virtual Collection Accounts to your system.

In order to commence testing, you may contact your Client Success Manager via [support@netbank.ph ](mailto:support@netbank.ph)or an alternative communication point that they have established for you to receive assistance during integration. Your client Success Managers would ask you for

1. The UAT bank account account (that you have opened via the Netbank Virtual Partner Dashboard) where your assigned 5-digit Partner Alias would be associated and 

1. A URL endpoint that you have exposed where we can push credit notification webhooks.

To simulate incoming transfers in the UAT environment, kindly utilize Netbank’s [Process Account-To-Account Fund Transfer](/docs#tag/Disburse-To-Account/operation/Disburse-To-Account_ProcessAccount-To-AccountFundTransfer) API. You can open and nominate another UAT bank account and designate it as your source account.

Feel free to reach out to your Client Success Managers for assistance on testing and other questions.
