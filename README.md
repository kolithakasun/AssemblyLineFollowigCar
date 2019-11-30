✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶
Assembly Line Followig Car

✦ Line following robot two wheeler car build using a Arduino uno (ATMega328p) and coded using Assembly Language
✦ This robort is created used two wheel robort car,
Created using,

	✦ Arduino UNO board ( Atmega328p )
	✦ Two wheel (2W) chassy
	✦ L298 motor drive
	✦ 3x TCRT 5000 Line following sensor
	
✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶

In this code, I used 

	◆ PORTB 3, 4, 5 pins for Line following sensors Left, Middle, Right respectively.
	◆ PORTB 0, 1 pins used for 2 enablers in the L298 motor drive. Left and Right respectively.
	◆ PORTD 2, 3, 4, 5 pins used for inputs of the motor drive (Input 1,2,3,4 respectively)
	

Following code is only the line following and does not have the other configurations. All configuration codes and  complte code included in the Line Following_Assembly_Code.asm file.
	
✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶✶
	
;-- Line Following Code
	
	HERE:
		SBIC PINB,4
		JMP CHECK3
		JMP Check_PIN3
	CHECK3: 
		SBIC PINB,3
		JMP CHECK5
		JMP Forward
	CHECK5:	
		SBIC PINB,5
		JMP STOP

	Check_PIN3:	
		SBIC PINB,3
		JMP TurnLeft
		SBIC PINB,5
		JMP TurnRight
;-- Code for Controls

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
	ST:	JMP ST
