# OpenSSL

## Generate a self-signed certificate

```
openssl genrsa -out key.pem
openssl req -new -key key.pem -out csr.pem
openssl x509 -req -days 9999 -in csr.pem -signkey key.pem -out cert.pem
rm csr.pem
```

Generation of RSA Private Key. Superseded by genpkey(1):

```
openssl genrsa -out key.pem
```

PKCS#10 X.509 Certificate Signing Request (CSR) Management.

```
openssl req -new -key key.pem -out csr.pem
```

X.509 Certificate Data Management:

```
openssl x509 -req -days 9999 -in csr.pem -signkey key.pem -out cert.pem
```
