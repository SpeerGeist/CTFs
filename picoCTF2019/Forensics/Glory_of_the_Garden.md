# Glory of the Garden - 50 points
Forensics

## Description:
> This garden contains more than it seems.

## Hint:
> What is a hex editor?

### Files in shell: garden.jpg

## Solving:

Trying to do
```bash
cat garden.jpg
```
will return a bunch of hex data that is unreadable. However the strings command will provide us with a lot of useless text with the flag sat at the end. 
```bash
strings garden.jpg | grep -oE 'picoCTF{.*?}'
```
returns the flag.

### Flag:
```
picoCTF{more_than_m33ts_the_3y3cD8bA96C}
```

