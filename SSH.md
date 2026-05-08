---
title: "SSH"
---

# how to gen key

``` sh1
ssh-keygen -t rsa -b 4096
ssh-keygen -t dsa 
ssh-keygen -t ecdsa -b 521
ssh-keygen -t ed25519
```

# how to copy key to remote

```commonlisp
ssh-copy-id -i ~/.ssh/tatu-key-ecdsa user@host
```

隧道

get public key from private key ,#+begin<sub>src</sub> sh ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIG4aMEni1mj+nTjregYWUMSADfjFBHiCNhxLiEt2s9+5 NixOps VPN key of bttc

```
#+end_src
```
