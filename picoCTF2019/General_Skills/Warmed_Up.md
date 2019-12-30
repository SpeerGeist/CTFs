# Warmed Up - 50 points
General Skills

## Description:
> What is 0x3D (base 16) in decimal (base 10).

## Hint:
> Submit your answer in our competition's flag format. For example, if you answer was '22', you would submit 'picoCTF{22}' as the flag.

## Solving:
Instead of putting it through a converter I used this in bash (taking out the '0x'):
```bash
echo 'obase=10; ibase=16; 3D' | bc
61
```


### Flag:
```
picoCTF{61}
```

