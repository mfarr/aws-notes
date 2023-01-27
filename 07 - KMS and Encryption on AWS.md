Note: ($) = Exam Tip

## KMS 101

* Managed service that makes it easy to create and control the encryption keys used to encrypt your data
* Seamlessly integrated with many AWS services to make encrypting data in those services as easy as checking a box
* Use whenever dealing with sensitive information
* CMK - Customer Master Key
	* Encrypt/decrypt data up to 4km
	* ($) Generates/encrypt/decrypt the **Data Key**
		* Data Key used to encrypt/decrypt actual data
	* Alias, Creation Date, Description, Key State (enabled, disabled, pending deletion, unavailable), Key Material (customer or AWS provided)
	* Stays inside KMS, can't be exported
	* IAM users and roles to admin and use the key through KMS API
* ($) AWS-Managed CMK
	* AWS-provided and managed CMK. Used on your behalf with the AWS services integrated with KMS
* ($) Customer-Managed CMK
	* You create, manage and own yourself

## KMS API Calls ($)

* aws kms encrypt
	* Encrypts plaintext into ciphertext by using a customer master key
* aws kms decrypt
	* Decrypts ciphertext that was encrypted by an AWS KMS customer master key (CMK)
* aws kms re-encrypt
	* Decrypts ciphertext an then re-encrypts it entire within the AWS KMS (eg. when you change CMK or manually rotate the CMK)
* aws kms enable-key-rotation
	* Enables automatic key rotation every year
* aws kms generate-data-key
	* Uses the CMK to generate a data key to encrypt data > 4KB

## Exploring Envelope Encryption

* ($) CMK used to encrypt the data (envelope) key
	* Data key encrypts our data **(anything over 4KB)**
* ($) GenerateDataKey API call to create a data key from CMK
* Why use envelope encryption?
	* ($) Network Performance - only the data key goes over the network, not all data that needs to be encrypted
	* Data encrypted locally without travelling over the network