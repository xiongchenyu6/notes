---
title: "openssl"
date: 2022-01-13
---

# Request for public cert #openssl #@crypto

```bash
openssl genpkey -genparam -algorithm ec -pkeyopt ec_paramgen_curve:P-256 -out ECPARAM.pem
openssl req -newkey ec:ECPARAM.pem -keyout PRIVATEKEY.key -out MYCSR.csr
```

# compare cert public key and public key from private key

```bash
openssl x509 -pubkey -in SERVER.cert -noout
openssl pkey -pubout -in PRIVATEKEYNOPASS.key     
```

# remove passphrase

```bash
openssl ec -in xxx -out xxx
```

# 自生成key以及签名

```bash
openssl req -new -x509 -subj "/C=SG/CN=fingerone.com" \
                  -addext "subjectAltName = DNS:fingerone.com" \
                  -addext "certificatePolicies = 1.2.3.4" \
                  --keyout localhost.pem -out localhost.pem -days 365 -nodes
```

```bash
openssl req -new -x509 -subj "/C=SG/CN=fingertwo.com" \
                  -addext "subjectAltName = DNS:fingertwo.com" \
                  -addext "certificatePolicies = 1.2.3.4" \
                  --keyout localhost2.pem -out localhost2.pem -days 365 -nodes
```

```bash
openssl req -new -x509 -subj "/C=SG/CN=riskcontrol.com" \
                  -addext "subjectAltName = DNS:riskcontrol.com" \
                  -addext "certificatePolicies = 1.2.3.4" \
                  --keyout riskcontrol.pem -out riskcontrol.pem -days 365 -nodes
```

```bash
openssl pkcs12 -export -out identity.pfx -inkey riskcontrol.pem -in riskcontrol.pem -certfile riskcontrol.pem
```
