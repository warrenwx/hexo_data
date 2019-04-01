---
title: Memory-Layout-C
date: 2018-08-22 20:42:02
tags:
---

#Memory-layout-of-variable-in-C-program

1. As you know, variable in C can be classified info local variable, global variable. Local variable is stored in Stack, memory allocated with malloc/alloc locates in Heap. Global variable and Static Variable is located in global data area, which consists of DS and BSS. DS (data segment) contains initialized variable, which is not zero. DS can be divided into readonly DS area and read write DS area. BSS (block started by symbol) contains uninitialized variable or variable initialized with zero, which can also be divied into readonly BSS area and read write area.

2.Sample

void ppp(int parameter) {
	
	return;
}

int main() {
	ppp(12);
	return 0;
}

REG	BEGIN		PUSH EBP	MOV EBP, ESP
EBP	0036FD88	0036FD88	0036FD74	main in
ESP	0036FD78	0036FD74	0036FD74	

REG	BEGIN		PUSH EBP	MOV EBP, ESP
EBP	0036FD74	0036FD74	0036FC9C	ppp in
ESP	0036FCA0	0036FC9C	0036FC9C

REG	BEGIN		MOV ESP, EBP	POP EBP
EBP	0036FC9C	0036FC9C	0036FD74	ppp return
ESP	0036F9D0	0036FC9C	0036FCA0	

REG	BEGIN		MOV ESP, EBP	POP EBP	
EBP	0036FD74	0036FD74	0036FD88	main return
ESP	0036FD74	0036FD74	0036FD78

Before the program enter main procudure, EBP is 0x0036FD88, so this is the Base Address of current Call Frame, ESP is bigger than EBP, I think all of us can undersand this in X86 Cpu architecture. Now, we enter main, I think I should save the Base Address, right now. As we know, Context should be saved and recovered when key operations begins and stops. So 0x0036FD88 is save to stack, then ESP is decreased to 0x0036FD74. And the same time, EBP is decreased to ESP (0x0036FD84).