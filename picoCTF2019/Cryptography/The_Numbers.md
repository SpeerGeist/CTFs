# The Numbers - 50 points
Cryptography

## Description:
> The numbers... what do they mean?

## Hint:
> The flag is in the format PICOCTF{}

### Files in shell: the_numbers.png


## Solving:

```bash
eog the_numbers.png
```
The image contains the numbers in the following format:
```
16 9 3 15 3 20 6 { 20 8 5 14 21 13 2 5 18 19 13 1 19 15 14 }
```

By knowing that the flag is in the format picoCTF{....} we can see it's just a A1Z26 cipher and using a site like https://planetcalc.com/4884/ we can input this and get:
```bash
p i c o c t f { t h e n u m b e r s m a s o n }
----------------------------------------------------------------------------------
echo p i c o c t f { t h e n u m b e r s m a s o n } | tr a-z A-Z | tr  -d ' '
PICOCTF{THENUMBERSMASON}
```

### Flag:
```
PICOCTF{THENUMBERSMASON}
```
