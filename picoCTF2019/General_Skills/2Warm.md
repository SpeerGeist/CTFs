# 2Warm - 50 points
General Skills

## Description:
> Can you convert the number 42 (base 10) to binary (base 2)? 

## Hint:
> Submit your answer in our competition's flag format. For example, if you answer was '11111', you would submit 'picoCTF{11111}' as the flag.

## Solving:
Exactly like 'Warmed_Up' we can modify this bash command to go from dec to bin:
```bash
echo 'obase=2; ibase=1; 42' | bc
101010
```


### Flag:
```
picoCTF{101010}
```

