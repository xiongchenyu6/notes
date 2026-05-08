---
title: "Apl"
id: "91091F87-E013-412D-9DEE-60AFDF9F1316"
date: "2022-02-08 Tue 00:08"
---

# algo

## buzz fizz

``` gnu-apl
{∊(3↑(0=3 5|⍵)∪1)/'Fizz' 'Buzz'⍵}¨⍳30
```

## Trapping Rain Water

``` gnu-apl
solution ← {+/((⌈\⍵)⌊⌽⌈\⌽⍵)-⍵}
```
