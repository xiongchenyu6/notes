---
title: "regex"
date: 2021-12-01
---

# Syntax

+? \*? ?? non-hungry match

# lib

## [[Python]]

match == search(\^)

``` python
import re
regex = "o"
print(re.match(regex, "dog"))
print(re.search(regex, "dog"))
return re.search(regex, "dog")
```

```text
<re.Match object; span=(1, 2), match='o'>
```

``` python
import re
regex = "o"
return re.match(regex, "dog")
```

```text
None
```

``` python
import re
regex = ".*o"
return re.match(regex, "dog")
```

```text
<re.Match object; span=(0, 2), match='do'>
```

``` python
import re
regex = "\W+"
return re.split(regex, "Words, words, words.dog.", 2)
```

  ------- ------- ------------
  Words   words   words.dog.
  ------- ------- ------------

``` scala
println("test")
```
