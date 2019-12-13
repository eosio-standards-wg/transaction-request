transaction-request: EOSIO Transaction request for QR and NFC
=============================================================


EOSIO Standards Working Group, 2019

https://github.com/eosio-standards-wg

Status: early draft

EDIT: this proposal is superceeded by EEP-7, so all implementation effort should be concentrated on it: https://github.com/greymass/eosio-signing-transport


Introduction
------------

Modern mobile phones are able to read QR codes and NFC tags without
any additional software, so it's reasonable to utilize them in EOSIO
mobile wallets.

This standard proposal aims to develop a common standard for mobile
wallets so that a payment or other transaction can be requested via QR
code or an NFC tag regarddless of what kind of wallet software is
running on the user device.




Requirements
------------

A QR code or NFC tag will open an HTTPS URL. This URL can also be
passed to the client directly, like in email message.

The wallet software should only accept HTTPS URLs with valid SSL
certificates.

The wallet should issue an HTTPS GET request to the URL, and it must
not pass any user identification to the remote server.

The response should have a special MIME type (TODO: propose one) that
is distinguished as transaction request.

The content should be in JSON format with fields as follows:

* `document-type`: `transaction-request`

* `standard-revision`: `1.0`

* `blockchain-standard`: `eosio`

* `chain-id`: hex string (64 characters) indicating the Chain ID

* `auth-user-substitute`: a string that the wallet needs to substitute
  with authorizing user account (Suggestion: `%%AUTHUSER%%`)

* `actions`: an array of JSON objects which represent a transaction for
  executing. The format is identical to that of `transact` function
  arguments in `eosjs 20.x`. The sender account is substituted with
  the string specified in `auth-user-substitute`.

* `description-short`: a one-line short description in plain text.

* `description-long`: an HTML string with a limited set of allowed HTML
  tags (TODO: define allowed tags).

* `feedback-url` (optional): an HTTPS URL under the same domain name
  as the request URL.

Upon receiving the document, the wallet software should substitute the
user account in actions, and provide a prompt to the user, showing all
3 fields in a readable form: `description-short`, `description-long`,
`actions`.

If `feedback-url` is present, the wallet software must display also
that the transaction request includes a feedback to the requestor.

Once the user agrees to send the transaction, it is signed by the
user's active key and broadcasted.

If `feedback-url` is present, the wallet software sends an HTTPS GET
request to the specified URL, by adding transaction ID as follows:
`?txid=TRANSACTIONID`


Donations
---------

https://github.com/eosio-standards-wg/donations


Copyright and License
---------------------

Copyright 2019 EOSIO Standards Working Group

This work is licensed under a Creative Commons Attribution 4.0
International License.

http://creativecommons.org/licenses/by/4.0/


