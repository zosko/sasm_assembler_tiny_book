````
=============================================
========  Assembler Tiny Book  ==============
=============================================
===========  Author: Bosko ==================
=============================================


[--------------------------------------------]
[----------- BYTES REPREZENTATION -----------]
[--------------------------------------------]

db -  8-bit representation, allocates 1 byte
dw - 16-bit representation, allocates 2 bytes
dd - 32-bit representation, allocates 4 bytes
dq - 64-bit representation, allocates 8 bytes

MSB - Most significant bit 

LSB - Least significant bit 


 MSB        8-bit            LSB
 -------------------------------
| 1 | 0 | 0 | 0 | 0 | 1 | 0 | 1 |
 -------------------------------
 ^^^
 SIGN 
 BIT

if SIGN BIT is 1 number is negative
current number above is negative.

NEGATIVE Binary convert to Decimal

1 0 1 0 1 0 0 0  ; original number

0 1 0 1 0 1 1 1  ; invert original number
              1  ; add 1
---------------
  1 0 1 1 0 0 0 = -88


[--------------------------------------------]
[---------------    BITS     ----------------] 
[--------------------------------------------]

-------------------    ------------------    -------------------
| X | Y | X and Y |    | X | Y | X or Y |    | X | Y | X xor Y |
-------------------    ------------------    -------------------
| 0 | 0 |    0    |    | 0 | 0 |    0   |    | 0 | 0 |    0    |
-------------------    ------------------    -------------------
| 0 | 1 |    0    |    | 0 | 1 |    1   |    | 0 | 1 |    1    |
-------------------    ------------------    -------------------
| 1 | 0 |    0    |    | 1 | 0 |    1   |    | 1 | 0 |    1    |
-------------------    ------------------    -------------------
| 1 | 1 |    1    |    | 1 | 1 |    1   |    | 1 | 1 |    0    |
-------------------    ------------------    -------------------

[--------------------------------------------]
[-------------    ARRAYS    -----------------]
[--------------------------------------------]

array_1 db 0x50,0x10,0x70,0x30

---------------------------------------------------
| Address  | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 | HEX  |
---------------------------------------------------
| 00000020 | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 0x50 |
---------------------------------------------------
| 00000021 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0x20 |
---------------------------------------------------
| 00000022 | 0 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0x70 |
---------------------------------------------------
| 00000023 | 0 | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 0x30 |
---------------------------------------------------

array_1 db 0x50,0x10,0x70,0x30
------------------------------
to access on 0x70 : 2*1+20

array_2 dw 0x50,0x10,0x70,0x30
------------------------------
to access on 0x70 : 2*2+20

array_3 dd 0x50,0x10,0x70,0x30
------------------------------
to access on 0x70 : 2*4+20

array_4 dq 0x50,0x10,0x70,0x30
------------------------------
to access on 0x70 : 2*8+20

---------------------------------------------
-----------   SHIFTING BYTES    -------------
---------------------------------------------

   CF
---------------------------------------
|| 0 || 1 | 1 | 0 | 1 | 1 | 0 | 1 | 1 |   <---- Original
--------------------------------------- 
---------------------------------------
|| 1 || 1 | 0 | 1 | 1 | 0 | 1 | 1 | 0 |   <---- Shift left by 1 bit
---------------------------------------
---------------------------------------
|| 1 || 0 | 1 | 1 | 0 | 1 | 1 | 0 | 0 |   <---- Shift left by 2 bits
---------------------------------------
---------------------------------------
|| 0 || 1 | 1 | 0 | 1 | 1 | 0 | 0 | 0 |   <---- Shift left by 3 bits
---------------------------------------

mov al, 11000011b
shl al, 4         ; shift left by 4 bits, al will be 110000b

mov al, 11000011b
mov cl, 8
shl al, cl        ; shift left by 4 bits, al will be 0b

                                                                CF
                             ---------------------------------------
Original              ---->  | 1 | 1 | 0 | 1 | 1 | 0 | 1 | 1 || 0 ||   
                             ---------------------------------------
                             ---------------------------------------
Shift right by 1 bit  ---->  | 0 | 1 | 1 | 0 | 1 | 1 | 0 | 1 || 1 ||   
                             ---------------------------------------
                             ---------------------------------------
Shift right by 2 bits ---->  | 0 | 0 | 1 | 1 | 0 | 1 | 1 | 0 || 1 ||   
                             --------------------------------------- 
                             --------------------------------------- 
Shift right by 3 bits ---->  | 0 | 0 | 0 | 1 | 1 | 0 | 1 | 1 || 0 || 
                             ---------------------------------------   

mov al, 11001101b
shr al, 2         ; 110011

Arithmetic Right shift instead 0 is populate with first byte on right side
                                                                     CF
                                  ---------------------------------------
Original                   ---->  | 1 | 1 | 0 | 1 | 1 | 0 | 1 | 1 || 0 ||   
                                  ---------------------------------------
                                  ---------------------------------------
Arithmetic right by 1 bit  ---->  | 1 | 1 | 1 | 0 | 1 | 1 | 0 | 1 || 1 ||   
                                  ---------------------------------------
                                  ---------------------------------------
Arithmetic right by 2 bit  ---->  | 1 | 1 | 1 | 1 | 0 | 1 | 1 | 0 || 1 ||   
                                  ---------------------------------------
mov al, 11001101b
sar al, 2         ; 11110011


[--------------------------------------------]
[---------------  STACK  --------------------]  
[--------------------------------------------]

 Address

          |------------|
00000020  |            |
          |------------|
00000028  |            |
          |------------|     push  10   ; add to stack 10
00000030  |            |     pop  rax   ; get last value from stack and store to rax, value is 10
          |------------|     push 100   ; add to stack 100
00000038  |            |     push 200   ; add to stack 200
          |------------|
00000040  |            |
          |------------|
00000048  |    200     |  <---- RSP register pointer for last element in stack
          |------------|
00000050  |    100     |
          |------------|
00000058  | 0x12345634 |
          |------------|


section .data
  var dq 7

section .text
  push 800
  pop  rax
  
  mov rbx, 20
  push rbx         ; add to stack 20
  push qword[var]  ; add to stack 7

  pop rcx          ; store 7 to rcx
  pop qword[var]   ; store 20 to var


[--------------------------------------------]
[---------------  PROCEDURES  ---------------]  
[--------------------------------------------]


section .data
  action    db  "procedure is called",0
  PrintProc dq  0

section .text
global CMAIN

PrintString:
  PRINT_STRING action

  ret

CMAIN:
  
  ;  call label
  ;  call reg
  ;  call mem

  call PrintString

  mov  rax, PrintString
  call rax

  mov  qword[PrintProc], PrintString
  call qword[PrintProc]


  xor rax, rax
  ret


-------- NESTED PROCEDURES --------

section .data
  action    db  "Proc3 is called",0

section .text
global CMAIN

Proc3:
  PRINT_STRING action
  ret

Proc2:
  call Proc3
  ret

Proc1:
  call Proc2
  ret

CMAIN:
  
  call Proc1

  xor rax, rax
  ret

-------- PASSING PARAMS --------

section .data
gt        db  "Parametar 1 > Parametar 2", 0
issequ    db  "Parametar 1 <= Parametar 2", 0

section .text
global CMAIN

IsGreater:
  cmp rcx, rdx
  jg Greater

  mov rax, 0
  jmp End

Greater:
  mov rax, 1

End:
  ret

GreaterPrint
  PRINT_STRING lssequ
  jmp EndProgram

CMAIN:
  mov rcx, 1
  mov rdx, -5
  call IsGreater

  test rax, rax
  jnz GreaterPrint

EndProgram:
  xor rax, rax
  ret

[--------------------------------------------]
[----------- MACROS / DEFINCES --------------]
[--------------------------------------------]

%macro [NAME OF MACRO] [NUMBER OF PARAMETARS]
      some code here
%endmacro

%macro clear_rax 0
      xor rax, rax
%endmacro

%macro clear_reg 1
      xor %1, %1
%endmacro

%macro find_min 3
      mov %1, %2
      cmp %1, %3
      jl %%less    ; %% is used for macro labels

      mov %1, %3
%%less:

%endmacro

section .text

CMAIN:
  clear_rax               ; no parametars
  clear_reg rax           ; parametar rax
  find_min  rax, 1, 3     ; 1 will be stored in rax
  PRINT_DEC 8, rax
  ret


[--------------------------------------------]
[---------------  I/O MACRO  ----------------]
[--------------------------------------------]

section .data
asm    db "assembly",0
string times 10 db 0

section .text

  ; PRINT_DEC       size, data
  ; PRINT_UDEC      size, data
  ; PRINT_HEX       size, data
  ; PRINT_STRING    data
  ; NEWLINE

  mov rax, 100
  PRINT_DEC 8, rax     ; print 100
  NEWLINE
  PRINT_HEX 8, rax     ; print 0x64
  PRINT_STRING asm     ; print assembly

  ; GET_DEC       size, data
  ; GET_UDEC      size, data
  ; GET_HEX       size, data
  ; GET_STRING    data, maxsize
  ; GET_CHAR      size, register

  GET_DEC    8, rax    ;  add -1 in input window
  GET_HEX    8, rbx    ;  add 20 in input window
  PRINT_DEC  8, rax    ;  print -1
  NEWLINE
  PRINT_HEX  8, rbx    ;  print 20

  GET_STRING string, 10   ; add some text in input window
  PRINT_STRING string

  GET_CHAR rbx        ; input is saved to rbx

[--------------------------------------------]
[---------------  FLAGS  --------------------]
[--------------------------------------------]

CF - Carry Flag
* Activated when is value too large to store in register
mov al, 100
add al, 200 ; CF will be activated because 255 value is MAX value in 8bit register

OF - Overflow Flag
* Activated when value out of the range for register
mov al, -100
sub al,  100 ; OF will be activated because -128 is MAX value in 8bit register

SF - Sign Flag
* Activated when value is negative number
mov al,        0b 
or  al, 10000000b ; SF will be activated when MSB is 1

ZF - Zero Flag
* Activated when value is 0
mov al, 100
sub al, 100   ; ZF will be activated because 0 value

[--------------------------------------------]
[--------------    STRINGS     --------------]
[--------------------------------------------]

--------- MOVS - move string --------- 

section .data
  source           db    "assembly language", 0
  length          equ    $-source
  destination   times    length db 0

  ; source:       rsi
  ; destination   rdi

section .text
  cld                     ; clear direction flag 
  mov rsi, source
  mov rdi, destination
  mov rcx, length
  rep movsb               ; in destination is copied string


section .data
  source           dd    0x11111111, 0x22222222, 0x33333333, 0x44444444
  length          equ    $-source
  type            equ    4
  destination   times    length db 0

  ; source:       rsi
  ; destination   rdi

section .text
  cld                     ; clear direction flag 
  mov rsi, source
  mov rdi, destination
  mov rcx, length / type
  rep movsd               ; in destination is copied string

--------- STOS - store string --------- 

section .data
  name      db    "assembly language", 0
  length   equ    $-name

  ; destination   rdi

section .text
  cld                     ; clear direction flag
  mov rdi, name
  mov al, 'a'
  mov rcx, length - 1
  rep stosb               ; 'a' is copied to name x times
  mov byte[rdi], 0

section .data
  worldlist   dw    0x12, 0xAB, 0x45, 0x67
  count      equ    3

  ; destination   rdi

section .text
  cld                     ; clear direction flag
  mov rdi, worldlist
  mov rcx, count
  mov ax, 1
  rep stosw               ; '1' is copied to name x times
  mov byte[rdi], 0

--------- LODS - load string --------- 

section .data
  string    db   "programming", 0
  length   equ   $-string

  ; source: rsi

section .text
  cld                    ; clear direction flag
  mov rsi, string
  mov rdi, rsi
  mov rcx, length-1

convert:
  lodsb                 ; load first character to 'al'

  sub al, 32            ; if you substrack 32 from char you get UPPERCASE char
  stosb                 ; store back to memory "string"

  loop convert          ; loop similar like JUMP, loop until rcx is 0

--------- SCAS - search for char in string --------- 

section .data
  string      db  "assembly language", 0
  length     equ  $-string
  found       db  0

; destination: rdi

; search for char in the string

section .text
  cld                    ; clear direction flag
  mov al, 'b'
  mov rdi, string
  mov rcx, length
  repne scasb            ; repetitve not equal

  jnz NotFound

  mov byte[found], 1
  jmp End
NotFound:
  mov byte[found], 0
End:
  end program

--------- CMPS - compare strings --------- 

section .data
  string1   db   "abcdefgh",0
  string2   db   "abcdefggh",0
  length   equ   $-string2
  equal     db   0

; source1: rsi
; source2: rdi

section .text
  cld                 ; clear direction flag
  mov rsi, string1
  mov rdi, string2
  mov rcx, length
  repe cmpsb          ; complare untill rcx is 0

  jnz NotEqual        ; check ZF is cleared

  mov byte[equal], 1
  jmp End
NotEqual:
  mov byte[equal], 0

End:
  end program

section .data
  dwordlist1   dd 0x11111111, 0x22222222, 0x33333333
  dwordlist2   dd 0x11111111, 0x22222222, 0x44444444
  count       equ 3
  equal       equ 0

section .text
  cld                 ; clear direction flag
  mov rsi, dwordlist1
  mov rdi, dwordlist2
  mov rcx, count
  repe cmpsd
  jnz NotEqual

  mov byte[equal], 1
  jmp End
NotEqual:
  mov byte[equal], 0
End:
  end program

[--------------------------------------------]
[--------------  INSTRUCTIONS  --------------]
[--------------------------------------------]

----- MOV - set value to register -----
mov rax, 100 ; set 100 to rax
mov rbx, rax ; set rax to rbx, rbx is 100 also 100

----- XCHG - exchange data between -----
mov rax, 100 ; set 100 to rax
mov rbx, 200 ; set 200 to rbx
xchg rbx, rax ; exchange values between, rax is 200, rbx is 100

----- INC - increment value -----
mov rax, 100 ; set 100 to rax
inc rax      ; now rax is 101

----- DEC - decrement value -----
mov rax, 100 ; set 100 to rax
dec rax      ; now rax is 99

----- NEG - make negative value -----
mov rax, -10 ; set -10 to rax
neg rax      ; make 10 rax

mov rax, 10  ; set 10 to rax
neg rax      ; make -10 rax

----- ADD - add value -----
mov rax, 10  ; set 10 to rax
mov rbx, 20  ; set 20 to rbx
add rax, rbx ; rax is 30

----- SUB - subtract value -----
mov rax, 20   ; set 20 to rax
mov rbx, 1    ; set 1 to rbx
sub rax, rbx  ; rax is 19

----- AND - logic AND -----
mov rax, 10010001b
and rax, 01110011b ; set to rax 10001

----- OR - logic OR -----
mov rax, 11110001b
or rax, 11010001b ; set to rax 11110001

----- XOR - logic XOR -----
mov rax, 10110101b
xor rax, 10010101b ; set to rax 100000
xor rax, rax       ; set 0 to rax

----- NOT - invert binary -----
mov rax, 11110001b
not rax            ; set to rax 1110

----- MUL - multiply value -----

----------------------------------------------------------
| multiplier  | multiplicant  | upper-half |  lower-half |
----------------------------------------------------------
| mul 8-bit   |      al      ->     ah           al      |
| mul 16-bit  |      ax      ->     dx           ax      |
| mul 32-bit  |     eax      ->    edx          eax      |
| mul 64-bit  |     rax      ->    rdx          rax      |
----------------------------------------------------------

mov al, 10
mov bl, 20
mul al        ; al will be 200,  ah will be 0

mov ax, 0x100
mov bx, 0x200
mul bx        ; ax will be 0,  dx will be 0x200

----- DIV - divider value -----

-----------------------------------------------------------------
|   divisor  |  upper-half |  lower-half   quoitent   remainder |
-----------------------------------------------------------------
| div 8-bit  |     ah      |     al    ->    al          ah     |
| div 16-bit |     dx      |     ax    ->    ax          dx     |
| div 32-bit |    edx      |    eax    ->   eax         edx     |
| div 64-bit |    rdx      |    rax    ->   rax         rdx     |
-----------------------------------------------------------------

mov al, 20
mov bl, 2
div bl        ; al will be 10,  ah will be 0

mov dx, 0x40
mov ax, 0
mov bx, 0x100
div bx        ; dx will be 0,  ax will be 0x4000

xor edx, edx
mov eax, 203
mov ebx, 5
div ebx       ; rax is 40  rdx  is 3

----- IMUL - SIGN multiplier value -----

imul  8-bit        ; one operand
imul 16-bit        ; one operand
imul 32-bit        ; one operand
imul 64-bit        ; one operand

mov  rax, -100
mov  rbx, 50
imul rbx           ; value is -5000
--
imul reg, imm      ; two operand
imul reg, reg      ; two operand
imul reg, mem      ; two operand

section .data
value dq 100

mov   rax, 10
imul  rax, 5       ; rax is 50

mov  rbx, -20
imul rax, rbx      ; rax is -1000
imul rax, [value]  ; rax is -100000

--
imul reg, reg, imm ; three operand
imul reg, mem, imm ; three operand

mov  rbx, 30
imul rax, rbx, 20        ; rax is 600
imul rax, [value], 10    ; rax is 1000

----- IDIV - SIGN devider value -----

idiv mem
idiv reg

idiv  8-bit      ah      al   ->    al     ah
idiv 16-bit      dx      ax   ->    ax     dx 
idiv 32-bit     edx     eax   ->   eax    edx 
idiv 64-bit     rdx     rax   ->   rax    rdx

----- CBW -> check sign bit for 8-bit -----
mov  al, 40    ; set 40 to al
cbw            ; check SIGN flag 0 or 1, ah is 0
mov  bl, 10    ; set 10 to bl
idiv bl        ; devide al, value is 4 

----- CWD -> check sign bit for 16-bit -----
mov  ax, -200     ; set -200 to ax
cwd               ; check SIGN flag 0 or 1, dx is 1
mov  bx, 100      ; set 100 to bx
idiv bx           ; devide ax, value is -2

----- CDQ -> check sign bit for 32-bit -----
mov  eax, 6000     ; set 6000 to eax
cdq                ; check SIGN flag 0 or 1, edx is 0
mov  ebx, -6       ; set -6 t ebx
idiv ebx           ; devide eax, value is -1000

----- CQO -----
mov  rax, 307    ; set 307 to rax
cqo              ; check SIGN flag 0 or 1, rdx is 0
mov  rbx, 5      ; set 5 to rbx
idiv rbx         ; devide rax, value is 61, reminder rdx is 2 

----- TEST - AND operand but not store to register, and check ZF is set or not -----
mov  al, 101011b   
test al,   1000b   ; check if 4 bit is 0, ZF is not activated
test al,    100b   ; check if 3 bit is 0, ZF is activated
test al,    101b   ; check if 1 and 3 bit is 0, ZF is not activated
test al,  10100b   ; check if 3 and 5 bit is 0, ZF is activated

----- LEA - get address location and store to register -----
sum db 10
mov rax, [sum] ; store 10 to rax
lea rbx, [sum] ; store memory location to rbx
lea rcx, [rbx] ; store 10 to rcx

----- JMP - jump to somewhere -----
jmp equal ; jump to equal:

----- JZ - jump if ZF is activated -----
mov rax, 10  ; set 10 to rax
sub rax, 10  ; rax is 0
jz  zero     ; jump to zero if flag ZF is activated

----- JNZ - jump if not activated ZF -----
mov rax, 20  ; set 20 to rax
sub rax, 10  ; rax is 10
jz  not_zero ; jump to not_zero if flag ZF not activated

----- JC - jump if CF activated -----
mov bl, 200  ; set 200 to bl
add bl, 100  ; add 100 to bl = 300
jc  carry    ; jump to carry if flag CF is activated

----- JNC - jump if not activated CF  -----
mov bl, 200   ; set 200 to bl
add bl, 10    ; add 10 to bl = 210
jnc not_carry ; jump to not_carry if flag CF is not activated

----- JO - jump if OF activated -----
mov bl, -100 ; set -100 to bl
sub bl, 100  ; subtract -100 to bl = -200
jo  overflow ; jump to overflow if flag OF is activated

----- JNO - jump if not activated ZF  -----
mov bl, -100       ; set -100 to bl
sub bl, 10         ; subtract -10 to bl = -110
jo  not_overflow   ; jump to not_overflow if flag OF is not activated

----- JS - jump if SF activated -----
mov bl,        0b  ; set 0 to bl
or  bl, 10000000b  ; 
jo  sign           ; jump to sign if flag SF is activated

----- JNS - jump if not activated ZF  -----
mov bl,       10b   ; set 10 to bl
or  bl, 10000000b   ; 
jno sign            ; jump to sign if flag SF is activated

----- CMP - compare SIGNED or UNSIGNED -----
mov rax, 5  ; add 5 to rax
cmp rax, 1  ; compare rax with 1

----- JE - left operand == right operand -----
mov rax, 5
cmp rax, 5
ja equal      ; 5 == 5 jump to equal

----- JA / JNBE - left operand > right operand [UNSIGNED NUMBER] -----
mov rax, 5
cmp rax, 1
ja greater    ; 5 > 1 jump to greater

----- JAE / JNB - left operand >= right operand [UNSIGNED NUMBER] -----
mov rax, 5
cmp rax, 5
ja  greater  ; 5 > 5 will not jump to greater
jae equal    ; 5 >= 5 jump to equal

----- JB / JNAE - left operand < right operand [UNSIGNED NUMBER] -----
mov rax, 20
cmp rax, 50
jb  less      ; 20 < 50 jump to less

----- JBE / JNA - left operand <= right operand [UNSIGNED NUMBER] -----
mov rax, 20
cmp rax, 20
jb  less       ; 20 < 20 will not jump to less
jbe equal      ; 20 <= 20 will jump to equal

----- JG / JNLE - left operand > right operand [SIGNED NUMBER] -----
mov rax, 20
cmp rax, 15
jg  greater     ; 20 > 15 jump to greater

----- JGE / JNL - left operand >= right operand [SIGNED NUMBER] -----
mov rax, 15
cmp rax, 15
jg  greater     ; 15 > 15 will not jump to greater
jge greater     ; 15 >= 15 will jump to greater

----- JL / JNGE - left operand < right operand [SIGNED NUMBER] -----
mov rax, -10
cmp rax,  50
jl  less        ; -10 < 50 will not jump to less

----- JLE / JNG - left operand <= right operand [SIGNED NUMBER] -----
mov rax, -10
cmp rax, -10
jl  less         ; -10 < -10 will not jump to less
jle less         ; -10 <= -10 will jump to less

[--------------------------------------------]
[--------------    EXAMPLES    --------------]
[--------------------------------------------]

section .data
  size  dq   0
  x     dq  10
  y     dq   5
  z     dq  10

section .text

; if (size == 0)
;    size = 1;
; endif

if:
  mov rax, [size]
  ;test rax, rax      ; one type of checking
  cmp rax, 0          ; other type of checking
  jnz endif

  mov qword[size], 1
endif:

--------------------------------------------

; if (x > y)
;    x = 100;
; else 
;    y = 200;
; endif

if:
  mov rax, [x]
  cmp rax, [y]
  ja else

  mov qword[y], 100
  jmp endif
else:
  mov qword[x], 200

--------------------------------------------

; if (x > y && y > z)
;    x = 100;
;  endif
if:
  mov rax, [x]
  mov rbx, [y]
  cmp rax, rbx        ; x > y
  jle endif

  cmp rbx, [z]        ; y > z
  jle endif

  mov qword[x],100
endif:

--------------------------------------------

; while ( x <= y )
;    x++;
; endwhile
  x     dq  0
  y     dq  10

while:
   mov rax, [x]
   cmp rax, [y]
   jg endwile
   inc rax
   jmp while
endwhile:

--------------------------------------------

; do
;   x++;
; while (x <= y)

   mov rax, [x]
   mov rbx, [y]
dowhile:
   inc rax
   cmp rax, rbx
   jle dowhile
endwhile:
   mov [x], rax

--------------------------------------------

section .data
varA     dq    2
varB     dq   20
varC     dq   10
result   dq    0
divzero  db    0

; result = ( varA * -100 ) / ( varB - varC)

mov   rax, -100
imul  qword[varA]      ; rdx : rax = varA * -100

mov rbx, [varB]
sub rbx, [varC]        ; rbx = varB - varC
jz DivByZero

idiv rbx
mov [result], rax
mov byte[divzero], 0
jmp end

DivByZero:
  mov byte[divzero], 1

end: 
  end code

=============================================
================  EOF  ======================
=============================================
````
