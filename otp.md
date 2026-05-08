---
title: "otp"
date: 2022-12-11
---

```bash
oathtool -v --totp -d 6 12345678909876543210

export hex = ${echo 8f5483295745b11bd351 | xxd -r -p | base32}

qrencode -o user.png 'otpauth://totp/user@machine?secret=DK2DEFASV26A====
qrencode -t UTF8 'otpauth://totp/user@machine?secret=DK2DEFASV26A===='
```
