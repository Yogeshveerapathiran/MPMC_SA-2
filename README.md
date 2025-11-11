# Write an assembly language program in 8051 to generate a 5 ms delay using Timer 1 in Mode 1 and toggle an LED connected to Port 1.7 continuously.
## AIM:

To write an assembly language program in 8051 microcontroller to generate a 5 ms delay using Timer 1 in Mode 1 and continuously toggle an LED connected to Port 1.7.

## ALGORITHYM:

1.Start the program at address 0000H.

2.Select Timer 1, Mode 1 (16-bit timer) by loading TMOD with 10H.

3.Load Timer 1 registers (TH1, TL1) with the initial values corresponding to a 5 ms delay.

    For 12 MHz crystal → 1 µs per machine cycle

      Required count = 65536 − 5000 = EC78H

    So, TH1 = ECH and TL1 = 78H
4.Start the timer (SETB TR1).

5.Wait until TF1 = 1, which indicates timer overflow → 5 ms delay completed.

6.Stop the timer and clear TF1 for next cycle. 7.Toggle P1.7 using CPL P1.7 (you can view this bit change in Keil simulation window).

## PROGRAM:
```asm
ORG 0000H        

MAIN:
    MOV TMOD, #10H    ; Timer 1 in Mode 1 (16-bit timer)
    CLR P1.7          ; Initialize LED OFF

REPEAT:
    CPL P1.7          ; Toggle LED connected to Port 1.7
    ACALL DELAY       ; Call 5 ms delay
    SJMP REPEAT       ; Repeat forever

;------------------------------------
; Subroutine: 5 ms delay using Timer 1
;------------------------------------
DELAY:
    MOV TH1, #0ECH    ; Load high byte for 5 ms delay
    MOV TL1, #077H    ; Load low byte for 5 ms delay
    SETB TR1          ; Start Timer 1

WAIT:
    JNB TF1, WAIT     ; Wait until Timer 1 overflows
    CLR TR1           ; Stop Timer 1
    CLR TF1           ; Clear overflow flag
    RET               ; Return to main program

END
```
## OUTPUT:

<img width="1211" height="636" alt="image" src="https://github.com/user-attachments/assets/7f9582ef-9f50-4c97-8a7a-ce4be6e1ba91" />

## RESULT:
The simulation in Keil µVision shows that Port 1.7 toggles continuously with a 5 ms delay, successfully demonstrating timer-based delay generation
