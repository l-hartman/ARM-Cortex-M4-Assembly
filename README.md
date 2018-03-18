# ARM-Cortex-M4-Assembly

various ARM Assembly code snippets for reference.

## General Syntax

```
Label   Opcode  Operands    Comments

init    MOV     R0,#100     ; set table size
        BX      LR          ; function return
```

### Sample Cortex-M Instructions

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

### Simple Arithmetic
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

