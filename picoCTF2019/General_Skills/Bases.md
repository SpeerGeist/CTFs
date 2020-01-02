# Bases - 100 points
General_Skills

## Description:
> What does this bDNhcm5fdGgzX3IwcDM1 mean? I think it has something to do with bases.

## Hint:
> Submit your answer in our competition's flag format. For example, if you answer was 'hello', you would submit 'picoCTF{hello}' as the flag.

## Solving:

You could use a site that has multiple converters, however seeing this as base64 I made the below script to output the flag.
```python
#! /usr/bin/env Python3

import base64

print('Decoding base64 string: ')

s = input()

x = base64.standard_b64decode(s).decode()

print('\n\nApplying flag format...\n\n')
print('='*25)
print('picoCTF{' + x + '}')
print('='*25)
print('\n\n\n')
```
```
Decoding base64 string: 
bDNhcm5fdGgzX3IwcDM1


Applying flag format...


=========================
picoCTF{l3arn_th3_r0p35}
=========================
```



### Flag: 

```
picoCTF{l3arn_th3_r0p35}
```
