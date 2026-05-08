---
title: "Pandoc"
date: 2021-11-20
---

# pandoc

```bash
pandoc in.md --pdf-engine=lualatex \
         -V 'mainfont:DejaVuSerif' \
         -V 'sansfont:DejaVuSans' \
         -V 'monofont:DejaVuSansMono' \
         -V 'mathfont:TeXGyreDejaVuMath-Regular' \
         -o out.pdf
```
