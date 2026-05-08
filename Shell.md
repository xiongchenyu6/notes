---
title: "Shell"
date: 2021-11-20
---

# loop

```bash
for x in {1..3}
do
    echo $x
done
```

  ---
  1
  2
  3
  ---

loop spaced separated string

```bash
X="a b"
for i in $X
do
    echo $i
done
```

  ---
  a
  b
  ---

loop array

```bash
X=("a" "b")
for i in "${X[@]}"
do
    echo $i
done
```

  ---
  a
  b
  ---

join array

```bash
X=("a" "b")
for i in "${X[*]}"
do
    echo $i
done
```

```text
a b
```

# Condition

```bash
if [[ 2 -eq 1 ]]
then
    echo "2 equal 1"
elif [[ 1 -eq 1 ]]
then
    echo "1 equal 1"
else
    echo "not equal"
fi
```

```text
1 equal 1
```

## Predicates

- -n VAR - True if the length of VAR is greater than zero.
- -z VAR - True if the VAR is empty.
- STRING1 = STRING2 - True if STRING1 and STRING2 are equal.
- STRING1 != STRING2 - True if STRING1 and STRING2 are not equal.
- INTEGER1 -eq INTEGER2 - True if INTEGER1 and INTEGER2 are equal.
- INTEGER1 -gt INTEGER2 - True if INTEGER1 is greater than INTEGER2.
- INTEGER1 -lt INTEGER2 - True if INTEGER1 is less than INTEGER2.
- INTEGER1 -ge INTEGER2 - True if INTEGER1 is equal or greater than INTEGER2.
- INTEGER1 -le INTEGER2 - True if INTEGER1 is equal or less than INTEGER2.
- -h FILE - True if the FILE exists and is a symbolic link.
- -r FILE - True if the FILE exists and is readable.
- -w FILE - True if the FILE exists and is writable.
- -x FILE - True if the FILE exists and is executable.
- -d FILE - True if the FILE exists and is a directory.
- -e FILE - True if the FILE exists and is a file, regardless of type (node, directory, socket, etc.).
- -f FILE - True if the FILE exists and is a regular file (not a directory or device).
- -d Directory

# Flags

```bash
while getopts u:a:f: flag
do
    case "${flag}" in
        u) username=${OPTARG};;
        a) age=${OPTARG};;
        f) fullname=${OPTARG};;
    esac
done
echo "Username: $username";
echo "Age: $age";
echo "Full Name: $fullname";
```

  ----------- -------
  Username:   
  Age:        
  Full        Name:
  ----------- -------

# Substring

```bash
X="a.b.c.zip"
echo ${X%.*}
echo ${X%%.*}
echo ${X#*.}
echo ${X##*.}
echo ${X:0:1}
echo ${X:1:2}
```

  ---------
  a.b.c
  a
  b.c.zip
  zip
  a
  .b
  ---------

# Read line

```bash
confirmMessage() {
     read -p "$1 ([m]ac or [l]inux): " REPLY
     case $(echo $REPLY | tr '[A-Z]' '[a-z]') in
         m|mac) echo "mac" ;;
         *)     echo "linux" ;;
     esac
}
```

# check process running

if process not running the return code from ps is 0

```bash
if ps -p $PID > /dev/null
```

# backup

```bash
find .  -name '*.sol' -exec mv "{}" "{}.bak" \;
```
