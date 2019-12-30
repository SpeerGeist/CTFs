# caesar - 100 points
Cryptography

## Description:
> Decrypt this message. - picoCTF{dspttjohuifsvcjdpobqjtwtVk}

## Hint:
> caesar cipher tutorial

## Solving:

So with this, there are loads of online caesar brute force tools. But for fun here's a bash script I've put together for this challenge.
```bash
#!/bin/bash
read -p "Enter encoded caesar cipher: " message
read -p "How many iterations do you want to force? " number
message=${message#*"{"}
message=${message%"}"}
IN="${message,,}"
prefix="picoCTF{"
suffix="}"
printf "\n converting...\n \n ================ \n $message \n ================ \n \n"

for I in $(seq $number); do
    printf '+'$I': '$prefix; echo $IN$suffix | tr $(printf %${I}s | tr ' ' '.' )\a-z a-za-z
done
```
Resulting in...

```bash
Enter encoded caesar cipher: picoCTF{zolppfkdqeboryfzlktjxksyyl}
How many iterations do you want to force? 25

 converting...
 
 ================ 
 zolppfkdqeboryfzlktjxksyyl 
 ================ 
 
+1: picoCTF{apmqqglerfcpszgamlukyltzzm}
+2: picoCTF{bqnrrhmfsgdqtahbnmvlzmuaan}
+3: picoCTF{crossingtherubiconwmanvbbo}
+4: picoCTF{dspttjohuifsvcjdpoxnbowccp}
+5: picoCTF{etquukpivjgtwdkeqpyocpxddq}
+6: picoCTF{furvvlqjwkhuxelfrqzpdqyeer}
+7: picoCTF{gvswwmrkxlivyfmgsraqerzffs}
+8: picoCTF{hwtxxnslymjwzgnhtsbrfsaggt}
+9: picoCTF{ixuyyotmznkxahoiutcsgtbhhu}
+10: picoCTF{jyvzzpunaolybipjvudthuciiv}
+11: picoCTF{kzwaaqvobpmzcjqkwveuivdjjw}
+12: picoCTF{laxbbrwpcqnadkrlxwfvjwekkx}
+13: picoCTF{mbyccsxqdrobelsmyxgwkxflly}
+14: picoCTF{nczddtyrespcfmtnzyhxlygmmz}
+15: picoCTF{odaeeuzsftqdgnuoaziymzhnna}
+16: picoCTF{pebffvatgurehovpbajznaioob}
+17: picoCTF{qfcggwbuhvsfipwqcbkaobjppc}
+18: picoCTF{rgdhhxcviwtgjqxrdclbpckqqd}
+19: picoCTF{sheiiydwjxuhkrysedmcqdlrre}
+20: picoCTF{tifjjzexkyvilsztfendremssf}
+21: picoCTF{ujgkkafylzwjmtaugfoesfnttg}
+22: picoCTF{vkhllbgzmaxknubvhgpftgouuh}
+23: picoCTF{wlimmchanbylovcwihqguhpvvi}
+24: picoCTF{xmjnndiboczmpwdxjirhviqwwj}
+25: picoCTF{ynkooejcpdanqxeykjsiwjrxxk}
```

The only result looking somewhat in English is a +3 Caesar Cipher, our flag.

### Flag: 

```
picoCTF{crossingtherubiconwmanvbbo}
```
