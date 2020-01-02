# First Grep - 100 points
General_Skills

## Description:
> Can you find the flag in file? This would be really tedious to look through manually, something tells me there is a better way. 

## Hint:
> grep tutorial


## Solving:

After going on the shell server is imply used a bash command to cat and extract the flag with grep.
```bash
cat file | grep -oE 'picoCTF{.*?}'
```
```
picoCTF{grep_is_good_to_find_things_eda8911c}

```



### Flag: 

```
picoCTF{grep_is_good_to_find_things_eda8911c}

```
