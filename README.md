# vog_asm0
describes the vog_asm language and interpreter in c++ which will run it.

purpose of vog_asm language: to be a simple ("toy") assembly language so that I have something to compile actual vog to. Vog code will probably be turned into vog_asm with a python script.

vog_asm will have the following registers:

inst: the instruction pointer register
instbuffer: holds an instruction pointer, but does not control instruction flow. Will be used for returning from functions
ans (128 bit): will hold the result of the most recent operation
base: will point to the base of the current stack frame
top: points to the top of the stack
data*: points to a spot in memory.

All of the following registers will be used to hold data for binary or unary operations. Each register's size is indicated in it's name:
a64
a32
a16
a8
b64
b32
b16
b8


vog_asm language implies the stack grows notionally upward. 

the following will be vog_asm commands (commands are listed as the command word followed by the aguments to it, all arguments are unsigned 64 bit ints):

log str: str is a pointer. This command prints to the console the string that str points at
log_ans: prints the value in ans register to the console
malloc x: will allocate x bytes on the heap and place a pointer to them in the ans register
memcpy dest src count: copies the first count bytes from src to dest (dest and src are both pointers)
save dest count: copies the least significant count bytes out of the ans register to dest (dest is a pointer)

raise_top x: raises the top of the stack (the value in the "top" register) by x bytes
lower_top x: lowers the top of the stack (the value in the "top" register) by x bytes
above_top x: calculates the value of the address x bytes above the top of the stack and saves it in ans
below_top x: calculates the value of the address x bytes below the top of the stack and saves it in ans

raise_base: raises the base of the stackframe (the value in the "base" register) by x bytes
lower_base: lowers the base of the stackframe (the value in the "base" register) by x bytes
above_base x: calculates the value of the address x bytes above the base of the stackframe and saves it in data*
below_base x: calculates the value of the address x bytes below the base of the stackframe and saves it in data*

load reg: memcpy data from location pointed to by data* to the given register (enough data to fill the register)
loadlit reg val: stores val into register reg
loadfrom reg loc: memcpy data from loc to register reg (enough data to fill the register)
saveto reg loc: copies the data from the given register into the location pointed to by loc (all the data in the register)

The following are all binary arithmetic operations. They do what they sound like. They can only do an operation on two registers of the same size. The "A" series      register are always the first operand, the "B" series register is always the 2nd operand. Results are stored into the ans register of appropriate size.

+i{64, 32, 16, 8}  //this notation means +i64, +i32, +i16, and +i8 are all valid commands
-i{64, 32, 16, 8}
*i{64, 32, 16, 8}  //IDK why this line is italicized
/i{64, 32, 16, 8}

+u{64, 32, 16, 8}
-u{64, 32, 16, 8}
*u{64, 32, 16, 8}  //IDK why this line is italicized
/u{64, 32, 16, 8}

+f{64, 32}
-f{64, 32}
*f{64, 32}  //IDK why this line is italicized
/f{64, 32}


Binary bitwise operations:
and64
and32
and16
and8

or64
or32
or16
or8

xor64
xor32
xor16
xor8

<< A B: left bitshifts the value in register A by the amount indicated in register B
>> A B: same idea but right this time

~ reg: nots all the bits in the given register

Control flow
label "name":
jmp x:
jmp_pos x:
jmp_neg x:
jmp_0 x:
jmp_!0 x:
ret: 



