# So Meta - 150 points
Forensics

## Description:
> Find the flag in this picture. You can also find the file in /problems/so-meta_1_ab9d99603935344b81d7f07973e70155.

## Hint:
> What does meta mean in the context of files?
> Ever hear of metadata?

## Solving:

After accessing the shell server we see the image 'pico_img.png'. Since this challenge suggests the flag is in the image metadata I used exiftool and bash to quickly extract it.
```bash
exiftool pico_img.png | grep -oE 'picoCTF{.*?}'

picoCTF{s0_m3ta_368a0341}

```

### Flag: 

```
picoCTF{s0_m3ta_368a0341}
```
