use these steps to generate key

# generate private/public keypair
openssl req -newkey rsa:2048 -nodes -keyout private_key.pem -x509 -days 365 -out certificate.pem

# write certificate in binary file (some sytems need binary format)
openssl x509 -outform der -in certificate.pem -out public_key.der

# get the public key from the certificate
openssl x509 -in certificate.pem -pubkey > public_key.pem

# import certificate into Java Key Store (JKS)
# !!! Be sure to trust the certificate - otherwise it's not imported
keytool -importcert -file certificate.pem -keystore keystore.jks -alias mycertificate -storetype jks

# create a PKCS12 keystore with private/public keypair
openssl pkcs12 -inkey private_key.pem -in certificate.pem -export -out keystore.p12 -name mykey

# import keypair into Java keystore
keytool -importkeystore -destkeystore keystore.jks -srckeystore keystore.p12 -srcstoretype pkcs12 -destalias mykey -srcalias mykey
