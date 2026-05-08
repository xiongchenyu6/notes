---
title: "GNUPG"
date: 2021-11-20
---

# Setup new key

```bash
export KEY_NAME="office.trontech.link"
export KEY_COMMENT="flux secrets"

gpg --batch --full-generate-key <<EOF
%no-protection
Key-Type: 1
Key-Length: 4096
Subkey-Type: 1
Subkey-Length: 4096
Expire-Date: 0
Name-Comment: ${KEY_COMMENT}
Name-Real: ${KEY_NAME}
EOF

gpg --list-secret-keys "${KEY_NAME}"

export KEY_FP="A27D 0937 F9E0 95BB 9B24  7B49 778B 8150 7612 BF48"


gpg --export-secret-keys --armor "${KEY_FP}" |
kubectl create secret generic sops-gpg \
--namespace=flux-system \
--from-file=sops.asc=/dev/stdin

gpg --delete-secret-keys "${KEY_FP}"
```

# Setup permission

```bash
find ~/.gnupg -type f -exec chmod 600 {} \; # Set 600 for files
find ~/.gnupg -type d -exec chmod 700 {} \; # Set 700 for directories
```

# GNUPG

## Encryption

Use recipient public key to encrypt message so only recipient can decrypt and view it ![](clipboard-20250617T142342.png)

### decrypt

``` shell
gpg --decrypt a.sh.gpg
gpg -q --pinentry-mode loopback --for-your-eyes-only --no-tty -d ~/.password/gmail.gpg
```

### encrypt

``` shell
gpg --encrypt -r person@email.com name_of_file
gpg --encrypt name_of_file
```

## Sign

Use sender private key to encrypt message so everyone can verify the sender identity ![](clipboard-20250617T142323.png)

```bash
gpg -b sample.txt
gpg --verify sample.txt.sig sample.txt
gpg --verify sample.txt.sig # save folder
```

since \*.sig file is not readable more commonly use clear text signature shown as below

```bash
gpg --clear-sign sample.txt
gpg --verify sample.txt.asc
```

### Sign string

```bash
echo "ee7d52053138e716702261914a330059ed470b1106b75569b18326e9c1e04d27" | gpg -a --default-key 5AF7AFBF695E8A5D --detach-sig
```

## Signature alongside encrypt

![](clipboard-20250617T142237.png)

```bash
gpg -s sample.txt # sign sample.txt.sig
gpg --verify a.sh.gpg
gpg --decrypt a.sh.gpg
```

## Transfer key

### exportq

``` shell
gpg --export --armor > public.key
gpg --export-secret-keys > private.key
gpg --export-secret-subkeys --armor > sub_private.key
gpg --export-ownertrust > ownertrust.txt
```

### delete

```bash
gpg –delete-secret-keys
gpg –delete-key
```

### import

```bash
gpg --import your@id.here.pub.asc
gpg --import your@id.here.priv.asc
gpg --import your@id.here.sub_priv.asc
gpg --import-ownertrust ownertrust.txt
```

### trust imported key

```bash
gpg --edit-key your@id.here
gpg> trust
Your decision? 5 (Ultimate trust)
```

### change expire date

change primary key then secondary

```bash
gpg --list-keys
gpg --edit-key xiongchenyu
g> expire
g> key 1
g> expire

```
