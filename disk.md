---
title: "disk"
date: 2023-08-10
---

fsck.ext4 -cvf

growpart /dev/nvme0n1 2 resize2fs /dev/nvme0n1p2
