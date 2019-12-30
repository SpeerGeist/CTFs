# Lets Warm Up - 50 points
General Skills

## Description:
> If I told you a word started with 0x70 in hexadecimal, what would it start with in ASCII? 

## Hint:
> Submit your answer in our competition's flag format. For example, if you answer was 'hello', you would submit 'picoCTF{hello}' as the flag.

## Solving:
Quick bash code to convert hex to ASCII:

```bash
echo 0x70 | xxd -r -p
p
```
### Flag:
```
picoCTF{p}
