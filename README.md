# ARM-Cortex-M4-Assembly

ARM Assembly code snippets for reference.


## General Syntax

```
Label   Opcode  Operands    Comments

init    MOV     R0,#100     ; set table size
        BX      LR          ; function return
```

### Nonsensical Sample Instructions
```
        MOV   R1,#100   ; R1=100
        MOV   R2,R1     ; R1= R2 (put a copy of R1 into R2)
        LDR   R1,=Count ; R1 points to the variable Count
        LDR   R0,[R1]   ; R0= value pointed to by R1
        STR   R2,[R1]   ; [R1]=R2 (store R0 to memory at [R1])
        ADD   R2,R0     ; R2= R2+R0
        ADD   R2,R0,R1  ; R2= R0+R1
        SUB   R1,R1,#100 ; R1= R1-100
        SUB   R2,R0,R1  ; R2= R0-R1
        LSL   R0,R1,#4  ; R0=R1<<4 (shift to the left by four positions)
        LSR   R2,R1,#3  ; R2=R1>>3 (shift to the right by 3 positions)
```

## Simple Algerbra
* compute: Vy = 7 * Vx, assuming Vx is in a register and Vy is a variable in memory.

```
        MOV   Vx,#12    ; initialize Vx
        MOV   R5,Vx     ; make copy of Vx in R5
        LSL   R5,R5,#3  ; R5= 8*R5=8*Vx
        SUB   R5,R5,Vx  ; R5= R5-Vx=8*Vx=7*Vx
        LDR   R0,=Vy    ; R0= address of Vy
        STR   R5,[R0]   ; Vy= R5
```

* compute: voltage = 9*current – 25

```
        AREA   MyData, DATA, READWRITE, ALIGN=2
current        RN      R7    ;
voltage        DCB     0     ;

        AREA   MyCode, CODE, READONLY, ALIGN=2
        EXPORT __main      
__main
        MOV     current,#15   ; initialize current
        MOV     R5,current    ; make copy of current in R5
        LSL     R5,R5,#3      ; R5 <- 8*R5=8*current
        ADD     R5,R5,current ; R5 <- 8*current + current = 9*current
        SUB     R5,R5,#25     ; R5 <- 9*current - 25
        LDR     R0,=voltage   ; R0 <- address of voltage
        STR     R5,[R0]       ; voltage <- R5
        END
```
## Number Stuff!
### BCD Digit Separation
* Assume that X = 25 (BCD), we want to get R1 = 2 and R2 = 5, and then store them in Xten and Xunit, respectively.

```
        MOV     R1,X            ; R1 gets a copy of X
        MOV     R2,R1           ; R2 gets a copy of X
        AND     R1,#0x0000000F  ; R1 has the units digit
        LDR     R0,=Xunit       ; R0 <- address of Xunit
        STRB    R1,[R0]         ; stores R1 in Xunit
        LSR     R2,#4           ; R2 has the tens digit
        LDR     R0,=Xten        ; R0 <- address of Xten
        STRB    R2,[R0]         ; stores R2 in Xten
```

### Covert BCD -> Binary
* 68<sub>10</sub> = (0110 1000)<sub>BCD</sub> = 0110 * 10<sup>1</sup> + 1000 * 10<sup>0</sup>
```
        THUMB
        AREA    MyData, DATA, READWRITE, ALIGN=2
bcdVal          RN      R7    ; Input number is 23 (Decimal)
bcdBin          DCB     0     ; Initialize Xunit to 0
       
        AREA    MyCode, CODE, ALIGN=2
        EXPORT __main
__main
        MOV     R1,bcdVal      ; R1<-bcdVal
        MOV     R2,R1          ; R2<-bcdVal
        AND     R1,#0x0000000F ; R1 has the units digit
        LSR     R2,#4          ; R2 has the tens digit
        MOV     R3,R2          ; R3 gets a copy of R2
        LSL     R2,#1          ; R2 gets 2*Xten
        ADD     R2,R3,LSL #3   ; R2 gets 2*Xten+8*Xten=10*Xten
        ADD     R2,R1          ; R2 gets 10*Xten + Xunit
        LDR     R0,=bcdBin     ; R0<-address of bcdBin
        STRB    R2,[R0]        ; store R2 in bcdBin
        END
```
## Branching and Control Flow
### branching
```
        B{cond}  label  ; branch to label
        BX{cond} Rm     ; branch indirect to location specified by Rm
        BL{cond} label  ; branch to subroutine at label
        BLX{}    Rm     ; branch to subroutine indirect specified by Rm
```
### conditionals
```
        
```
