openssl closed_lock_with_key
Install

Install the OpenSSL on Debian based systems

sudo apt-get install openssl

Commands

Create a private key

openssl genrsa -out server.key 4096

Generate a new private key and certificate signing request

openssl req -out server.csr -new -newkey rsa:4096 -nodes -keyout server.key

Generate a self-signed certificate

openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:4096 -keyout server.key -out server.crt

Generate a certificate signing request (CSR) for an existing private key

openssl req -out server.csr -key server.key -new

Generate a certificate signing request based on an existing certificate

openssl x509 -x509toreq -in server.crt -out server.csr -signkey server.key

Remove a passphrase from a private key

openssl rsa -in server.pem -out newserver.pem

Parse a list of revoked serial numbers

openssl crl -inform DER -text -noout -in list.crl

Check a certificate signing request (CSR)

openssl req -text -noout -verify -in server.csr

Check a private key

openssl rsa -in server.key -check

Check a public key

openssl rsa -inform PEM -pubin -in pub.key -text -noout
openssl pkey -inform PEM -pubin -in pub.key -text -noout

Check a certificate

openssl x509 -in server.crt -text -noout
openssl x509 -in server.cer -text -noout

Check a PKCS#12 file (.pfx or .p12)

openssl pkcs12 -info -in server.p12

Verify a private key matches an certificate

openssl x509 -noout -modulus -in server.crt | openssl md5
openssl rsa -noout -modulus -in server.key | openssl md5
openssl req -noout -modulus -in server.csr | openssl md5

Display all certificates including intermediates

openssl s_client -connect www.paypal.com:443

Convert a DER file (.crt .cer .der) to PEM

openssl x509 -inform der -in server.cer -out server.pem

Convert a PEM file to DER

openssl x509 -outform der -in server.pem -out server.der

Convert a PKCS#12 file (.pfx .p12) containing a private key and certificates to PEM

openssl pkcs12 -in server.pfx -out server.pem -nodes

Convert a PEM certificate file and a private key to PKCS#12 (.pfx .p12)

openssl pkcs12 -export -out server.pfx -inkey server.key -in server.crt -certfile CACert.crt

Generate a Diffie Hellman key

openssl dhparam -out dhparam.pem 2048

Encrypt files with rsautl

openssl rsautl -encrypt -in plaintext.txt -out encrypted.txt -pubin -inkey pubkey.pem

Decrypt files with rsautl

openssl rsautl -decrypt -in encrypted.txt -out plaintext.txt -inkey privkey.pem
