;; AssemblyLineFollowigCar
;; Line following robot two wheeler car build using a Arduino uno (ATMega328p) and coded using Assembly Language

CODE:

;
; AssemblerApplication22.asm
;
; Created: 11/1/2019 5:08:25 AM
; Author : kasun
;
;


; Replace with your application code
 
.ORG 0x0 ;location for reset 
  JMP MAIN

   .ORG 0x0E ;ISR location for Timer2 compare match A
  JMP TIMER2_A

  .ORG 0x10 ;ISR location for Timer2 compare match B
  JMP TIMER2_B

  .ORG 0x1C ;ISR location for Timer0 compare match A
  JMP TIMER0_A

  .ORG 0x1E ;ISR location for Timer0 compare match B
  JMP TIMER0_B

;main program
MAIN:
  LDI R20,HIGH(RAMEND)
  OUT SPH,R20
  LDI R20,LOW(RAMEND)
  OUT SPL,R20;set up stack 

  SBI DDRB,0;PB5 as an output
  SBI DDRB,1;PB5 as an output
  SBI DDRD,4
  SBI DDRD,5
  SBI DDRD,2
  SBI DDRD,3
  
  CBI DDRB,3	;Sensors L
  CBI DDRB,4	;Sensors M
  CBI DDRB,5	;Sensors R
  
;-- Initializing TImer0
  LDI R20,255
  OUT OCR0A,R20 	;load Timer0 with 239
  LDI R20,5 
  out OCR0B,R20
  
;--Initializing TImer2
  LDI R20,255
  STS OCR2A,R20
  LDI R21,5
  STS OCR2B,R21

  LDI R20,0x02
  OUT TCCR0A,R20
  LDI R20,0x01
  OUT TCCR0B,R20 ;start Timer0, CTC mode, no scaler

  LDI R20,0x02
  STS TCCR2A,R20
  LDI R20,0x01
  STS TCCR2B,R20 ;start Timer0, CTC mode, no scaler

  LDI R20,0x06
  STS TIMSK0,R20

  LDI R20,0x06
  STS TIMSK2,R20


  cBI PORTD,4
  cBI PORTD,5

  SBI PORTD,2
  SBI PORTD,3
 
  SEI ;set I (enable interrupts globally)
  JMP HERE
 
;-- Line Following Code
HERE:
 
SBIC PINB,4
	JMP CHECK3
	JMP Check_PIN3
CHECK3: SBIC PINB,3
		JMP CHECK5
		JMP Forward
CHECK5:SBIC PINB,5
		JMP STOP

Check_PIN3:	
SBIC PINB,3
			JMP TurnLeft
SBIC PINB,5
			JMP TurnRight

TurnLeft:
	LDI R20,120 
	OUT OCR0B,R20
	LDI R20,85 
	STS OCR2B,R20
	JMP HERE
Forward:
	LDI R20,88 
	OUT OCR0B,R20
	LDI R20,88
	STS OCR2B,R20
	JMP HERE

TurnRight:
	LDI R21,120 
	STS OCR2B,R21
	LDI R20,85 
	OUT OCR0B,R20
	JMP HERE
	
STOP:
	LDI R21,200 
	STS OCR2B,R21
	LDI R20,200 
	OUT OCR0B,R20
ST: JMP ST

;--ISR for Timer0
T0_CM_ISR:
  CBI PORTB,0
  RETI ;return from interrupt

T0_CM1_ISR:
  SBI PORTB,0
  RETI ;return from interrupt

;--ISR for Timer 2
T2_CM_ISR:
  CBI PORTB,1
  RETI ;return from interrupt

T2_CM1_ISR:
  SBI PORTB,1
  RETI ;return from interrupt

