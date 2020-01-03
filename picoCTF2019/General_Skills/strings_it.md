# strings it - 100 points
General Skills

## Description:
> Can you find the flag in file without running it? 

## Hint:
> strings

## Solving:

Once connected to the shell server we find a file called 'strings' and using the strings bash command along with grep as below we extract the flag. 
```bash
strings strings | grep -oE picoCTF{.*?}

picoCTF{5tRIng5_1T_c7fff9e5}

```
### Flag: 

```
picoCTF{5tRIng5_1T_c7fff9e5}
```
