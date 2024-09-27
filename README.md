<style>
mark{
    color:red;
}
</style>
<style>
.errorCodes tr:nth-child(4) td:nth-child(1) { background: #C9DAF8; }
.errorCodes tr:nth-child(6) td:nth-child(1) { background: #C9DAF8; }
.errorCodes tr:nth-child(7) td:nth-child(1) { background: #C9DAF8; }
.errorCodes tr:nth-child(8) td:nth-child(1) { background: #C9DAF8; }
.errorCodes tr:nth-child(14) td:nth-child(1) { background: #C9DAF8; }
</style>

# GENERAL GUIDELINES

## INTRODUCTION

Welcome to the Netbank Virtual API Technical Documentation.

This documentation/reference includes all the details regarding the API endpoints and webhooks that will allow you to integrate Netbank’s financial services into your own applications, systems, and platforms.

We strongly suggest that you read through the General Guidelines section to familiarize yourself with the overall structure and behavior of the Netbank Virtual APIs.

The API endpoints are grouped according to the product and are ordered based on the usual call sequence for easier reference. Each product, endpoint, and parameter will have a description to define the purpose of each element.

Disclaimer: We will continually update and enhance this page and its contents whenever necessary.

To know more about how to get started and the end-to-end onboarding process, you may refer to https://virtual.netbank.ph/get-started

For any questions/inquiries, you may refer to our FAQs (https://virtual.netbank.ph/faq) or reach out to the Netbank Virtual Team via the Contact Us Form found in various pages of https://virtual.netbank.ph/ and select “Request for integration support for Sandbox and UAT development” in the the “How can we help you?” dropdown.

## DEVELOPMENT ENVIRONMENTS

There are 3 environments where you could use our APIs for specific purposes.

- <b>SANDBOX (https://api-sandbox.netbank.ph)</b>
	- <b><i>Description:</b></i> This environment is where you can “try” or test the request and response of our APIs to have an idea on the format and behaviour that your system needs to integrate with.

	- <b><i>Purpose of Use:</b></i> To simulate and test the request and response payload of the APIs so you could finalize how your system interacts with our APIs.

	- <b><i>Connection to our Core:</b></i> These APIs are connected to a limited are of our Core Banking System but it mimics how our APIs would accept requests and return a response.

	- <b><i>Access Credentials:</b></i> You can quickly and simply generate Sandbox Credentials using the Credentials Management section of your Partner Dashboard.
	
		- Sign-In and access the Netbank Virtual Partner Dashboard
		
		- Navigate to the Credentials Management section
	
		- Click “Generate Client ID & Secret”
	
		- Select “Sandbox Environment”
	
		- Take note of your Client ID and Client Secret

	- <b><i>Requirements to get access:</b></i>

		- Netbank Virtual Account (Sign Up to Netbank Virtual)

		- Sandbox Access Credentials, (Generated via the Credentials Management section of the Partner Dashboard)

- <b>UAT (https://api-uat.netbank.ph)</b>

	- <b><i>Description:</b></i> This environment is where you can perform a deeper level of testing and review to ensure that your integration with us is ready for Production-use.

	- <b><i>Purpose of Use:</b></i> To perform various quality assurance testing (e.g. unit, stress, performance, acceptance testing) to ensure that the integration can handle all types of Production-use scenarios.

	- <b><i>Connection to our Core:</b></i> These APIs are connected to the test environment of our Core Banking System that reflects the same level of capacity and behaviour as the Production environment.

	- <b><i>Access Credentials:</b></i> You can simply generate UAT Credentials using the Credentials Management section of your Partner Dashboard but we suggest using this environment solely for quality assurance testing-- which is done after you’ve finalized the integration setup to our APIs.

		- Sign-In and access the Netbank Virtual Partner Dashboard

		- Navigate to the Credentials Management section

		- Click “Generate Client ID & Secret”

		- Select “UAT Environment”
	
		- Fill out the Request for UAT Credentials form

		- Take note of your Client ID and Client Secret

	- <b><i>Requirements to get access:</b></i>

		- Netbank Virtual Account (Sign Up to Netbank Virtual)

		- UAT Access Credentials (Generated via the Credentials Management section of the Partner Dashboard)

		- Information on the product/service that you are building

- <b>PROD (https://api.netbank.ph)</b>

	- <b><i>Description:</b></i> This environment is where you can create live accounts and initiate live transactions.

	- <b><i>Purpose of Use:</b></i> To use the Netbank products and services in actual business transactions and use cases.

	- <b><i>Connection to our Core:</b></i> These APIs are connected to the production environment of our Core Banking System.

	- <b><i>Access Credentials:</b></i> You can request for Production Credentials using the Credentials Management section of your Partner Dashboard along with the Pre-Production requirements.
	
		- Sign-In and access the Netbank Virtual Partner Dashboard
	
		- Navigate to the Credentials Management section
	
		- Click “Generate Client ID & Secret”
	
		- Select “PROD Environment”
	
		- Fill out the Request for PROD Credentials form
	
		- Wait for Netbank to Approve your Prod Credentials Request
	
		- Take note of your Client ID and Client Secret

	- <b><i>Requirements to get access:</b></i>

		- Netbank Virtual Account (Sign Up to Netbank Virtual)

		- Prod Access Credentials (Generated via the Credentials Management section of the Partner Dashboard)

		- Information on the product/service that you are building

		- Banking-As-A-Service License Agreement (Signed)

		- Any other relevant agreements (Signed)
	
		- UAT Sign-Off (to indicate that you’ve fully tested the integration in all possible scenarios and that it’s ready for Production-Use)

## AUTHENTICATION

The Netbank Virtual APIs utilizes OAuth 2.0–a popular and widely used protocol–to authenticate the API requests. Please use this documentation as guide on how to implement the authentication flow: https://oauth.net/2/

- <b>Access Token</b>

	- the Netbank APIs require an Access Token to process any type of API request (GET, POST, PUT).

	- An Access Token can be generated after successfully requesting for authorization and getting an Authorization Grant.

	- The Netbank Virtual Access Token has a default validity of 1 hour. (Netbank can grant long-lived tokens depending on the risk assessment during onboarding)

- <b>Authorization Grant</b>

	Netbank Virtual supports 2 ways (or “Grant Types”) to get an Authorization Grant depending on the transaction scenario and the  type of authorization needed

	- <b>If the API User is trying to access/use a Netbank Bank Account that he/she owns,</b>

		- the API User needs to request for authorization directly from Netbank’s Authorization Server

		- the type of Authorization Grant to be used would be the “<b>Client Credentials</b>” (Client ID and Client Secret). Please use this documentation as guide on how to implement the authentication flow: https://oauth.net/2/

		- The Client ID and Secret for this type of Authorization Grant  can be generated by accessing the Partner Dashboard and navigating to the “Credentials Management” > “Environment Access Credentials” > “Generate Client ID & Secret”

	- <b>If the API User is trying to access/use a Netbank Bank Account of another entity/individual, </b>

		- the API User needs to gather consent and request authorization directly from the Netbank account holder by directing the account holder to Netbank’s Authorization Server

		- the type of Authorization Grant to be used would be the “<b>Authorization Code</b>” (Client ID and Client Secret). Please use this documentation as guide on how to implement the authentication flow: https://oauth.net/2/

		- The Client ID and Secret for this type of Authorization Grant  can be generated by accessing the Partner Dashboard and navigating to the “Credentials Management” > “Authentication Code” > “Generate Client ID & Secret”. The form needs to be filled out:

			- <b>Name:</b> Name/Label for your credentials

			- <b>Environment:</b> The development environment that you would use these credentials for. (“Sandbox”, “UAT”, “PROD”)

			- <b>CORS URL:</b> Describe which URL are permitted to read authorization information from a web browser

			- <b>Redirect URL:</b> The url where the user will be redirected after a successful authorization and where the Access Token will be posted

		- An end user's first-time multi-factor authentication (MFA) flow will consist of a three-step Security question, One Time Password (OTP) through SMS and Consent confirmation. A session cookie will stored in the user’s browser and will be valid for 365 days. 	

		- When users have to do re-authentication they will directly go to the OTP page if the session cookie is still valid. If no cookie is present we default to first-time MFA flow.	

- <b>Authorization URLs</b>
	
	- <b>Auth Base URL</b>: https://auth.netbank.ph

	- <b>Authorization URL</b>: https://auth.netbank.ph/oauth2/auth?userid={{customer_id of the account holder}}
	
	- <b>Token URL</b>:  https://auth.netbank.ph/oauth2/token
	
	- <b>Scopes</b>

		- Execute a Transaction:  https://auth.netbank.bnk.to/transaction.write
	
		- Offline Access: offline_access

		- OpenID Connect: openid

## ERROR HANDLING

During negative scenarios, our APIs would provide 3 identifiers to indicate what the issue is. We suggest to pattern your error handling based on these levels of reference:

- <b>HTTP Response Status Code</b>: This indicates the standard response of an HTTP request.

- <b>Error Code</b>: these are the well defined status codes that gRPC uses.

- <b>Error Message</b>: this will indicate the error description coming from our core system.

## REASON FOR REJECTION

- <b>Settlement Rails Error Code</b>

ISO 20022 is a global standard for exchanging electronic messages between financial institutions. There
are a lot of different messaging formats in the financial section so it is essential to make use of a common
standard to get everyone on the same page.


| CODE | LABEL | DEFINITION |
| -- | --| -- |
| AC01 | IncorrectAccountNumber | Format of the account number specified is not correct |
| AC02 | InvalidDebtorAccountNumber | Debtor account number invalid or missing |
| AC03 |InvalidCreditorAccountNumber | Wrong IBAN in SCT |
| AC04 | ClosedAccountNumber | Account number specified has been closed on the bank of account's books |
| AC05 | ClosedDebtorAccountNumber | Debtor account number closed |
| AC06 | BlockedAccount | Account specified is blocked, prohibiting posting of transactions against it.|
| AC07 | ClosedCreditorAccountNumber | Creditor account number closed |
| AC08 | InvalidBranchCode | Branch code is invalid or missing |
| AC09 | InvalidAccountCurrency | Account currency is invalid or missing |
| AC10 | InvalidDebtorAccountCurrency | Debtor account currency is invalid or missing |
| AC11 | InvalidCreditorAccountCurrency | Creditor account currency is invalid or missing |
| AC12 | InvalidAccountType | Account type missing or invalid Generic usage if cannot specify between group and payment information levels |
| AC13 | InvalidDebtorAccountType | Debtor account type is missing or invalid |
| AC14 | InvalidAgent | An agent in the payment chain is invalid. |
| AG01 | TransactionForbidden | Transaction forbidden on this type of account (formerly NoAgreement) |
| AG02 | InvalidBankOperationCode | Bank Operation code specified in the message is not valid for receiver |
| AG03 | TransactionNotSupported | Transaction type not supported/authorized on this account |
| AG04 | InvalidAgentCountry | Agent country code is missing or invalid Generic usage if cannot specify between group and payment information levels |
| AG05 | InvalidDebtorAgentCountry | Debtor agent country code is missing or invalid |
| AG06 | InvalidCreditorAgentCountry | Creditor agent country code is missing or invalid |
| AG07 | UnsuccesfulDirectDebit | Debtor accounts cannot be debited for a generic reason. Code value may be used in general purposes and as a replacement for AM04 if debtor bank does not reveal its customer's insufficient funds for privacy reasons |
| AG08 | InvalidAccessRights | Transaction failed due to invalid or missing user or access right |
| AGNT | IncorrectAgent | Agent in the payment workflow is incorrect |
| AM01 | ZeroAmount | Specified message amount is equal to zero |
| AM02 | NotAllowedAmount | Specific transaction/message amount is greater than allowed maximum |
| AM03 | NotAllowedCurrency | Specified message amount is a non processable currency outside of existing agreement |
| AM04 | InsufficientFunds | Amount of funds available to cover the specified message amount is insufficient. |
| AM05 | Duplication | Duplication |
| AM06 | TooLowAmount | Specified transaction amount is less than agreed minimum |
| AM07 | BlockedAmount | Amount of funds available to cover the specified message amount is insufficient. |
| AM09 | WrongAmount | Amount received is not the amount agreed or expected |
| AM10 | InvalidControlSum | Sum of instructed amounts does not equal the control sum. |
| AM11 | InvalidTransactionCurrency | Transaction currency is invalid or missing|
| AM12 | InvalidAmount | Amount is invalid or missing |
| AM13 | AmountExceedsClearingSystemLimit | Transaction amount exceeds limits set by clearing system |
| AM14 | AmountExceedsAgreedLimit | Transaction amount exceeds limits agreed between bank and client |
| AM15 | AmountBelowClearingSystemMinimum | Transaction amount below minimum set by clearing system |
| AM16 | InvalidGroupControlSum | Control Sum at the Group level is invalid |
| AM17 | InvalidPaymentInfoControlSum | Control Sum at the Payment Information level is invalid |
| AM18 | InvalidNumberOfTransactions | Number of transactions is invalid or missing Generic usage if cannot specify between group and payment information levels |
| AM19 | InvalidGroupNumberOfTransactions | Number of transactions at the Group level is invalid or missing |
| AM20 | InvalidPaymentInfoNumberOfTransactions | Number of transactions at the Payment Information level is invalid |
| AM21 | LimitExceeded |Transaction amount exceeds limits agreed between bank and client.|
| ARDT | AlreadyReturnedTransaction | Already returned original SCT |
| ARPL | AwaitingReply | Reported when the cancellation request cannot be processed because no reply has been received yet from the receiver of the request message. |
| BE01 | InconsistenWithEndCustomer | Identification of the end customer is not consistent with associated account number (formerly CreditorConsistency). |
| BE04 | MissingCreditorAddress | Specification of creditor's address, which is required for payment, is missing/not correct (formerly IncorrectCreditorAddress).
| BE05 | UnrecognisedInitiatingParty| Party who initiated the message is not recognised bythe end customer |
| BE06 | UnknownEndCustomer | End customer specified is not known at associated Sort/National Bank Code or does no longer exist in the books |
| BE07 | MissingDebtorAddress | Specification of debtor's address, which is required for payment, is missing/not correct. |
| BE08 | BankError | Party who initiated the message is not recognised by the end customer Returned as a result of a bank error. |
| BE09 | InvalidCountry | Country code is missing or Invalid Generic usage if cannot specifically identify debtor or creditor |
| BE10 | InvalidDebtorCountry | Debtor country code is missing or Invalid |
| BE11 | InvalidCreditorCountry | Creditor country code is missing or Invalid |
| BE12 | InvalidCountryOfResidence | Country code of residence is missing or Invalid Generic usage if cannot specifically identify debtor or creditor | 
| BE13 | InvalidDebtorCountryOfResidence | Country code of debtor's residence is missing or Invalid |
| BE14 | InvalidCreditorCountryOfResidence | Country code of creditor's residence is missing or Invalid |
| BE15 | InvalidIdentificationCode | Identification code missing or invalid Generic usage if cannot specifically identify debtor or creditor| 
| BE16 | InvalidDebtorIdentificationCode | Debtor or Ultimate Debtor identification code missing or invalid |
| BE17 | InvalidCreditorIdentificationCode | Creditor or Ultimate Creditor identification code missing or invalid |
| BE18 | InvalidContactDetails | Contact details missing or invalid |
| BE19 | InvalidChargeBearerCode | Charge bearer code for transaction type is invalid |
| BE20 | InvalidNameLength | Name length exceeds local rules for payment type. |
| BE21 | MissingName | Name missing or invalid Generic usage if cannot specifically identify debtor or creditor |
| BE22 | MissingCreditorName | Creditor name is missing |
| CH03 | RequestedExecutionDateOrRequestedCollectionDateTooFarInFuture | Value in Requested Execution Date or Requested Collection Date is too far in the future |
| CH04 | RequestedExecutionDateOrRequestedCollectionDateTooFarInPast | Value in Requested Execution Date or Requested Collection Date is too far in the past |
| CH07 | ElementIsNotToBeUsedAtB-andC-Level | Element is not to be used at B- and C-Level |
| CH09 | MandateChangesNotAllowed | Mandate changes are not allowed |
| CH10 | InformationOnMandateChangesMissing | Information on mandate changes are missing |
| CH11 | CreditorIdentifierIncorrect | Value in Creditor Identifier is incorrect |
| CH12 | CreditorIdentifierNotUnambiguouslyAtTransaction-Level | Creditor Identifier is ambiguous at Transaction Level |
| CH13 | OriginalDebtorAccountIsNotToBeUsed | Original Debtor Account is not to be used |
| CH14 | OriginalDebtorAgentIsOnlyToBeUsedWithSequenceTypeFRST | Original Debtor Agent is only to be used with SequenceType=FRST |
| CH15 | ElementContentIncludesMoreThan140Characters | Content Remittance Information/Structured includes more than 140 characters |
| CH16 | ElementContentFormallyIncorrect | Content is incorrect |
| CH17 | ElementNotAdmitted | Element is not allowed |
| CH19 | ValuesWillBeSetToNextTARGETday | Values in Interbank Settlement Date or Requested Collection Date will be set to the next TARGET day |
| CH20 | DecimalPointsNotCompatibleWithCurrency | Number of decimal points not compatible with the currency |
| CH21 | RequiredCompulsoryElementMissing | Mandatory element is missing |
| CH22 | COREandB2BwithinOnemessage | SDD CORE and B2B not permitted within one message |
| CN01 | AuthorisationCancelled | Authorisation is canceled. |
| CNOR | Creditor bank is not registered | Creditor bank is not registered under this BIC in the CSM |
| CURR | IncorrectCurrency | Currency of the payment is incorrect |
| CUST | RequestedByCustomer|  Cancellation requested by the Debtor |
| DNOR | Debtor bank is not registered | Debtor bank is not registered under this BIC in the CSM |
| DS01 | ElectronicSignaturesCorrect | The electronic signature(s) is/are correct |
| DS02 | OrderCancelled | An authorized user has canceled the order |
| DS03 | OrderNotCancelled | The user’s attempt to cancel the order was not successful |
| DS04 | OrderRejected | The order was rejected by the bank side (for reasons concerning content) |
| DS05 | OrderForwardedForPostprocessing | The order was correct and could be forwarded for post processing |
| DS06 | TransferOrder | The order was transferred to VEU |
| DS07 | ProcessingOK | All actions concerning the order could be done by the EBICS bank server |
| DS08 | DecompressionError | The decompression of the file was not successful |
| DS09 | DecryptionError | The decryption of the file was not successful |
| DS0A | DataSignRequested | Data signature is required. |
| DS0B | UnknownDataSignFormat | Data signature for the format is not available or invalid. |
| DS0C | SignerCertificateRevoked | The signer certificate is revoked. |
| DS0D | SignerCertificateNotValid | The signer certificate is not valid (revoked or not active). |
| DS0E | IncorrectSignerCertificate | The signer certificate is not present. |
| DS0F | SignerCertificationAuthoritySignerNotValid | The authority of the signer certification sending the certificate is unknown. |
| DS0G | NotAllowedPayment | Signers are not allowed to sign this operation type. |
| DS0H | NotAllowedAccount | Signers are not allowed to sign for this account. |
| DS0K | NotAllowedNumberOfTransaction | The number of transactions is over the number allowed for this signer. |
| DS10 | Signer1CertificateRevoked | The certificate is revoked for the first signer. |
| DS11 | Signer1CertificateNotValid | The certificate is not valid (revoked or not active) for the first signer. |
| DS12 | IncorrectSigner1Certificate | The certificate is not present for the first signer.|
| DS13 | SignerCertificationAuthoritySigner1NotValid | The authority of signer certification sending the certificate is unknown for the first signer. |
| DS14 | UserDoesNotExist | The user is unknown on the server |
| DS15 | IdenticalSignatureFound | The same signature has already been sent to the bank |
| DS16 | PublicKeyVersionIncorrect | The public key version is not correct. This code is returned when a customer sends signature files to the financial institution after conversion from an older program version (old ES format) to a new program version (new ES format) without having carried out re-initialisation with regard to a public key change. |
| DS17 | DifferentOrderDataInSignatures | Order data and signatures don’t match |
| DS18 | RepeatOrder | File cannot be tested, the complete order has to be repeated. This code is returned in the event of a malfunction during the signature check, e.g. not enough storage space. |
| DS20 | Signer2CertificateRevoked | The certificate is revoked for the second signer. |
| DS21 | Signer2CertificateNotValid | The certificate is not valid (revoked or not active) for the second signer. |
| DS22 | IncorrectSigner2Certificate | The certificate is not present for the second signer. |
| DS23 | SignerCertificationAuthoritySigner2NotValid | The authority of signer certification sending the certificate is unknown for the second signer. |
| DS24 | WaitingTimeExpired | Waiting time expired due to incomplete order |
| DS25 | OrderFileDeleted | The order file was deleted by the bank server(for multiple reasons) |
| DS26 | UserSignedMultipleTimes | The same user has signed multiple times |
| DS27 | UserNotYetActivated | The user is not yet activated (technically) |
| DS28 | ReturnForTechnicalReason | Return following technical problems resulting in erroneous transactions. |
| DT01 | InvalidDate | Invalid date (eg, wrong or missing settlement date) |
| DT02 | InvalidCreationDate | Invalid creation date and time in Group Header (eg, historic date) |
| DT03 | InvalidNonProcessingDate | Invalid non bank processing date (eg, weekend or local public holiday) |
| DT04 | FutureDateNotSupported | Future date not supported |
| DT05 | InvalidCutOffDate | Associated message, payment information block or transaction was received after an agreed processing cut-off date, i.e., date in the past. |
| DT06 | ExecutionDateChanged | Execution Date has been modified in order for transaction to be processed |
| DU01 | DuplicateMessageID | Message Identification is not unique. | 
| DU02 | DuplicatePaymentInformationID | The Payment Information Block is not unique. |
| DU03 | DuplicateTransaction | Transactions are not unique. |
| DU04 | DuplicateEndToEndID | End To End ID is not unique. |
| DU05 | DuplicateInstructionID | Instruction ID is not unique. |
| DUPL | DuplicatePayment | Payment is a duplicate of another payment |
| ED01 | CorrespondentBankNotPossible | Corresponding bank is not not possible. |
| ED03 | BalanceInfoRequest | Balance of payments complementary info is requested |
| ED05 | SettlementFailed | Settlement of the transaction has failed. |
| EMVL | EMV Liability Shift | The card payment is fraudulent and was not processed with EMV technology for an EMV card.|
| ERIN | ERIOptionNotSupported | The Extended Remittance Information (ERI) option is not supported. |
| FF01 | Invalid | File Format File Format incomplete or invalid |
| FF02 | SyntaxError | Syntax error reason is provided as narrative information in the additional reason information. |
| FF03 | InvalidPaymentTypeInformation | Payment Type Information is missing or invalid Generica usage if cannot specify Service Level or Local Instrument code |
| FF04 | InvalidServiceLevelCode | Service Level code is missing or invalid |
| FF05 | InvalidLocalInstrumentCode | Local Instrument code is missing or invalid |
| FF06 | InvalidCategoryPurposeCode | Category Purpose code is missing or invalid |
| FF07 | InvalidPurpose | Purpose is missing or invalid |
| FF08 | InvalidEndToEndId | End to End Id missing or invalid |
| FF09 | InvalidChequeNumber | Cheque number missing or invalid |
| FF10 | BankSystemProcessingError | File or transaction cannot be processed due to technical issues at the bank side |
| FOCR | FollowingCancellationRequest | Return following a cancellation request |
| FR01 | Fraud | Returned as a result of fraud. |
| FRTRF | FinalResponseMandate | Canceled Final response/tracking is recalled as the mandate is canceled. |
| ID01 | CorrespondingOriginalFileStillNotSent | Signature file was sent to the bank but the corresponding original file has not been sent yet. |
| LEGL | LegalDecision | Reported when the cancellation cannot be accepted because of regulatory rules. |
| MD01 | NoMandate | No Mandate |
| MD02 | MissingMandatoryInformationIn | Mandate Mandate related information data required by the scheme is missing. |
| MD05 | CollectionNotDue | Creditor or creditor's agent should not have collected the direct debit |
| MD06 | RefundRequestByEndCustomer | Return of funds requested by end customer |
| MD07 | EndCustomerDeceased | End customer is deceased. |
| MS02 | NotSpecifiedReasonCustomerGenerated | Reason has not been specified by end customer |
| MS03 | NotSpecifiedReasonAgentGenerated | Reason has not been specified by agent. |
| NARR | Narrative | Reason is provided as narrative information in the additional reason information. |
| NOAS | NoAnswerFromCustomer | No response from Beneficiary |
| NOCM | NotCompliant | Customer accounts are not compliant with regulatory requirements, for example FICA (in South Africa) or any other regulatory requirements which render an account inactive for certain processing. |
| NOOR | NoOriginalTransactionReceived | Original SCT never received |
| PINL | PIN Liability Shift | The card payment is fraudulent (lost and stolen fraud) and was processed as an EMV transaction without PIN verification. |
| PTNA | PassedtoTheNextAgent | Reported when the cancellation request cannot be accepted because the payment instruction has been passed to the next agent. |
| RC01 | BankIdentifierIncorrect | The Bank Identifier code specified in the message has an incorrect format (formerly IncorrectFormatForRoutingCode).
| RC02 | InvalidBankIdentifier | Bank identifier is invalid or missing Generic usage if cannot specify between debit or credit account |
| RC03 | InvalidDebtorBankIdentifier | Debtor bank identifier is invalid or missing |
| RC04 | InvalidCreditorBankIdentifier | Creditor bank identifier is invalid or missing |
| RC05 | InvalidBICIdentifier | BIC identifier is invalid or missing Generic usage if cannot specify between debit or credit account |
| RC06 | InvalidDebtorBICIdentifier | Debtor BIC identifier is invalid or missing |
| RC07 | InvalidCreditorBICIdentifier | Creditor BIC identifier is invalid or missing |
| RC08 | InvalidClearingSystemMemberIdentifier | ClearingSystemMemberidentifier is invalid or missing Generic usage if cannot specify between debit or credit account |
| RC09 | InvalidDebtorClearingSystemMemberIdentifier | Debtor ClearingSystemMember identifier is invalid or missing |
| RC10 | InvalidCreditorClearingSystemMemberIdentifier | Creditor ClearingSystemMember identifier is invalid or missing |
| RC11 | InvalidIntermediaryAgent | Intermediary Agent is invalid or missing |
| RC12 | MissingCreditorSchemeId | Creditor Scheme Id is invalid or missing |
| RF01 | NotUniqueTransactionReference | Transaction reference is not unique within the message. |
| RR01 | Missing Debtor Account or Identification | Specification of the debtor’s account or unique identification needed for reasons of regulatory requirements is insufficient or missing |
| RR02 | Missing Debtor Name or Address | Specification of the debtor’s name and/or address needed for regulatory requirements is insufficient or missing. |
| RR03 | Missing Creditor Name or Address | Specification of the creditor’s name and/or address needed for regulatory requirements is insufficient or missing. |
| RR04 | Regulatory Reason | Regulatory Reason |
| RR05 | RegulatoryInformationInvalid | Regulatory or Central Bank Reporting information missing, incomplete or invalid. |
| RR06 | TaxInformationInvalid | Tax information missing, incomplete or invalid. |
| RR07 | RemittanceInformationInvalid | Remittance information structure does not comply with rules for payment type. |
| RR08 | RemittanceInformationTruncated | Remittance information truncated to comply with rules for payment type. |
| RR09 | InvalidStructuredCreditorReference | Structured creditor reference invalid or missing. |
| RR10 | InvalidCharacterSet | Character set supplied not valid for the country and payment type. |
| RR11 | InvalidDebtorAgentServiceID | Invalid or missing identification of a bank proprietary service. |
| RR12 | InvalidPartyID | Invalid or missing identification required within a particular country or payment type. |
| RUTA | ReturnUponUnableToApply | Return following investigation request and no remediation possible. |
| SL01 | Specific Service offered by Debtor Agent | Due to specific service offered by the Debtor Agent |
| SL02 | Specific Service offered by Creditor Agent | Due to specific service offered by the Creditor Agent |
| SL11 | Creditor not on Whitelist of Debtor | Whitelisting service offered by the Debtor Agent; Debtor has not included the Creditor on its “Whitelist” (yet). In the Whitelist the Debtor may list all allowed Creditors to debit Debtor bank accounts. |
| SL12 | Creditor on Blacklist of Debtor | Blacklisting service offered by the Debtor Agent; Debtor included the Creditor on his “Blacklist”. In the Blacklist the Debtor may list all Creditors not allowed to debit Debtor bank accounts. |
| SL13 | Maximum number of Direct Debit Transactions exceeded | Due to Maximum allowed Direct Debit Transactions per period service offered by the Debtor Agent. |
| SL14 | Maximum Direct Debit Transaction Amount exceeded | Due to Maximum allowed Direct Debit Transaction amount service offered by the Debtor Agent. |
| SP01 | PaymentStopped | Payment is stopped by the account holder. |
| SP02 | PreviouslyStopped | Previously stopped by means of a stop payment advice. |
| SVNR | ServiceNotRendered | The card payment is returned since a cash amount rendered was not correct or goods or a service was not rendered to the customer, e.g. in an ecommerce situation. |
| SYAD | RequestToSettlementSystemAdministrator | Cancellation requested by System Member to Settlement System Administrator to indicate that the cancellation request must not be forwarded further in the chain. |
| TA01 | TransmissonAborted | The transmission of the file was not successful – it had to be aborted (for technical reasons) |
| TD01 | NoDataAvailable | There is no data available (for download) |
| TD02 | FileNonReadable | The file cannot be read (e.g. unknown format) |
| TD03 | IncorrectFileStructure | The file format is incomplete or invalid |
| TECH | TechnicalProblem | Cancellation requested following technical problems resulting in an erroneous transaction. |
| TM01 | InvalidCutOffTime Formerly: CutOffTime | Associated message, payment information block or transaction was received after agreed processing cut-off time. |
| TRAC | RemovedFromTracking | Return following direct debit being removed from tracking process. |
| TS01 | TransmissionSuccessful | The (technical) transmission of the file was successful. |
| TS04 | TransferToSignByHand | The order was transferred to pass by accompanying note signed by hand
| UPAY | UnduePayment | Payment is not justified. |
| 9910 | Receiving Bank- Logged Off | Receiver signed-off |
| 9912 | Receiving Participant not available | Sending Participant sends a message where the recipient cannot be connected, receives a rejection |


## HTTP RESPONSE CODES
| Code | Label | Description |
| -- | --| -- |
| 200 | OK | Processing is successful |
| 400 | BAD REQUEST | The request payload has a missing parameter and/or invalid format |
| 403 | FORBIDDEN | The call does not have the proper authentication |
| 404 | NOT FOUND | The resource that is being retrieved is not existing |
| 500 | INTERNAL SERVER ERROR | There is an issue in processing the request |



## ERROR CODES

Netbank Virtual APIs use the gRPC status codes to further classify the success or failure of an API request.
**Those highlighted in <a style="background-color:#C9DAF8;">blue</a> are the codes that are mostly used by our APIs.

<div class="errorCodes">

| Code | Label | Description |
| -- | -- | -- |
| 0 | **OK** | Not an error; returned on success. |
| 1 | CANCELLED | The operation was cancelled, typically by the caller. |
| 2 | **UNKNOWN** | Unknown error. For example, this error may be returned when a Status value received from another address space belongs to an error space that is not known in this address space. Also errors raised by APIs that do not return enough error information may be converted to this error. |
| 3 | **INVALID_ARGUMENT** | The client specified an invalid argument. Note that this differs from FAILED_PRECONDITION. INVALID_ARGUMENT indicates arguments that are problematic regardless of the state of the system (e.g., a malformed file name) |
| 4 | DEADLINE_EXCEEDED | The deadline expired before the operation could complete. For operations that change the state of the system, this error may be returned even if the operation has completed successfully. For example, a successful response from a server could have been delayed long |
| 5 | **NOT_FOUND** | Some requested entity (e.g., file or directory) was not found. Note to server developers: if a request is denied for an entire class of users, such as gradual feature rollout or undocumented allowlist, NOT_FOUND may be used. If a request is denied for some users within a class of users, such as user-based access control, PERMISSION_DENIED must be used. |
| 6 | **ALREADY_EXISTS** | The entity that a client attempted to create (e.g., file or directory) already exists. |
| 7 | **PERMISSION_DENIED** | The caller does not have permission to execute the specified operation. PERMISSION_DENIED must not be used for rejections caused by exhausting some resource (use RESOURCE_EXHAUSTED instead for those errors). PERMISSION_DENIED must not be used if the caller can not be identified (use UNAUTHENTICATED instead for those errors). This error code does not imply the request is valid or the requested entity exists or satisfies other pre-conditions. |
| 8 | RESOURCE_EXHAUSTED | Some resource has been exhausted, perhaps a per-user quota, or perhaps the entire file system is out of space. |
| 9 | FAILED_PRECONDITION | The operation was rejected because the system is not in a state required for the operation's execution. For example, the directory to be deleted is non-empty, an rmdir operation is applied to a non-directory, etc. Service implementors can use the following guidelines to decide between FAILED_PRECONDITION, ABORTED, and UNAVAILABLE: (a) Use UNAVAILABLE if the client can retry just the failing call. (b) Use ABORTED if the client should retry at a higher level (e.g., when a client-specified test-and-set fails, indicating the client should restart a read-modify-write sequence). (c) Use FAILED_PRECONDITION if the client should not retry until the system state has been explicitly fixed. E.g., if an "rmdir" fails because the directory is non-empty, FAILED_PRECONDITION should be returned since the client should not retry unless the files are deleted from the directory. |
| 10 | ABORTED | The operation was aborted, typically due to a concurrency issue such as a sequencer check failure or transaction abort. See the guidelines above for deciding between FAILED_PRECONDITION, ABORTED, and UNAVAILABLE. |
| 11 | OUT_OF_RANGE | The operation was attempted past the valid range. E.g., seeking or reading past end-of-file. Unlike INVALID_ARGUMENT, this error indicates a problem that may be fixed if the system state changes. For example, a 32-bit file system will generate INVALID_ARGUMENT if asked to read at an offset that is not in the range [0,2^32-1], but it will generate OUT_OF_RANGE if asked to read from an offset past the current file size. There is a fair bit of overlap between FAILED_PRECONDITION and OUT_OF_RANGE. We recommend using OUT_OF_RANGE (the more specific error) when it applies so that callers who are iterating through a space can easily look for an OUT_OF_RANGE error to detect when they are done. |
| 12 | UNIMPLEMENTED | The operation is not implemented or is not supported/enabled in this service. |
| 13 | **INTERNAL** | Internal errors. This means that some invariants expected by the underlying system have been broken. This error code is reserved for serious errors. |
| 14 | UNAVAILABLE | The service is currently unavailable. This is most likely a transient condition, which can be corrected by retrying with a backoff. Note that it is not always safe to retry non-idempotent operations. |
| 15 | DATA_LOSS | Unrecoverable data loss or corruption. |
| 16 | UNAUTHENTICATED | The request does not have valid authentication credentials for the operation.|

</div>

## API TIMEOUT HANDLING AND IDEMPOTENCY

Idempotency simply refers to the API design principle that ensures that multiple identical API requests should return the same and original response of the initial request instead of reprocessing the operation. The Netbank Virtual APIs are designed to be idempotent in order to:

- <b>Avoid unintentional duplicate API requests</b>
  
	This protects our API users from the risk of unintentional reprocessing of transactions due to the mistake of re-sending the same API request twice or more.
  
- <b>Retrieve the API Response in the event of an API Request Timeout</b>

	This also serves as a mechanism to retrieve the original response of a request in the event that the API call has timed out due to network interruptions or other causes.

How does it work:

- Our <b>GET</b> APIs are idempotent by nature given that these APIs retrieve information or data from our database. As such, sending multiple identical requests will return the same response.

- Our <b>POST</b> and <b>PUT</b> APIs can be made idempotent by passing a unique id in the <b>idempotency-key</b> header parameter as part of the API request. Sending a request payload and a idempotency-key that is identical to a previous request and key would simply return the response of the original request without processing the second identical request.

- In case of an API request timeout, simply resend the same request with the same idempotency-key.

	- If the original request successfully pushed through and was processed in our system, the API would simply return the response of the original request.
	
	- If the original request did not reach our system, we would process the second request and return a response just like a regular API call.
	
## OTHER API GUIDELINES

- When you start to use our APIs either in UAT or PROD, we will create a dedicated “Branch” for you in our core banking system where all your customers, accounts, and transactions would be associated/recorded. This allows us to create a silo for your activities with us to keep it private, secure, and exclusive to your application.


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





