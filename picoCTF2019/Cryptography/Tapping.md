# Tapping - 200 points
Cryptography

## Description:
> Theres tapping coming in from the wires. What's it saying nc 2019shell1.picoctf.com 37920.

## Hint:
> What kind of encoding uses dashes and dots?
> The flag is in the format PICOCTF{}

## Solving:

For this challenge, again, we could have simply entered the recieved call on an online morse code translator. BUT AGAIN, for fun, I created a python script (using part of another morse code decoder) to call the host and return the code to translate. 

```c
import socket
import time


# Morse Code Dictionary
MORSE_CODE_DICT = { 'A':'.-', 'B':'-...',
   'C':'-.-.', 'D':'-..', 'E':'.',
   'F':'..-.', 'G':'--.', 'H':'....',
   'I':'..', 'J':'.---', 'K':'-.-',
   'L':'.-..', 'M':'--', 'N':'-.',
   'O':'---', 'P':'.--.', 'Q':'--.-',
   'R':'.-.', 'S':'...', 'T':'-',
   'U':'..-', 'V':'...-', 'W':'.--',
   'X':'-..-', 'Y':'-.--', 'Z':'--..',
   '1':'.----', '2':'..---', '3':'...--',
   '4':'....-', '5':'.....', '6':'-....',
   '7':'--...', '8':'---..', '9':'----.',
   '0':'-----', ', ':'--..--', '.':'.-.-.-',
   '?':'..--..', '/':'-..-.', '-':'-....-',
   '(':'-.--.', ')':'-.--.-', '{':'{', '}':'}',
}


# Calling on link and port
def netcat(hn, p):
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((hn, p))
    data = sock.recv(1024)
    sock.close()
    return data

def get_Host_name_IP():
    host_IP = socket.gethostbyname('2019shell1.picoctf.com')
    return(host_IP)
picoIp = get_Host_name_IP()
morse = (netcat(picoIp, 37920))
morse = str(morse, 'utf-8')

# Putting through decipher
def decryption(message):
   message += ''
   decipher = ''
   mycitext = ''
   for letter in message:
      # checks for space
      if (letter != ' '):
         i = 0
         mycitext += letter
      else:
         i += 1
         if i == 2:
            decipher += ' '
         else:
            decipher += list(MORSE_CODE_DICT.keys())[list(MORSE_CODE_DICT.values()).index(mycitext)]
            mycitext = ''
   return decipher

# Print out and call decipher
def main(morse):
    encoded = morse
    output = decryption(encoded)
    print('Connecting to ' + get_Host_name_IP() + ' on port ' + '37920')
    time.sleep(0.5)
    print(morse)
    print('\ntranslating...\n')
    time.sleep(1.3)
    print('Your flag is:')
    print(output)


# Execute main function
if __name__ == '__main__':
    main(morse)
```

Returns:

```c
Connecting to 3.15.247.173 on port 37920
.--. .. -.-. --- -.-. - ..-. { -- ----- .-. ... ...-- -.-. ----- -.. ...-- .---- ... ..-. ..- -. ...-- ----. -.... ----- ---.. ..... ....- ...-- ----. --... } 

translating...

Your flag is:
PICOCTF{M0RS3C0D31SFUN3960854397}
```
### Flag: 

```c
PICOCTF{M0RS3C0D31SFUN3960854397}
```
