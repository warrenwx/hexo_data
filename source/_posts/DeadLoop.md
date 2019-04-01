---
title: DeadLoop
date: 2018-08-22 20:41:25
tags:
---

###How to write simple deadloop, check following.

while(true){}

        .section        __TEXT,__text,regular,pure_instructions
        .macosx_version_min 10, 13
        .globl  _main                   ## -- Begin function main
        .p2align        4, 0x90
_main:                                  ## @main
        .cfi_startproc
## BB#0:
        pushq   %rbp
Lcfi0:
        .cfi_def_cfa_offset 16
Lcfi1:
        .cfi_offset %rbp, -16
        movq    %rsp, %rbp
Lcfi2:
        .cfi_def_cfa_register %rbp
        movl    $0, -4(%rbp)
LBB0_1:                                 ## =>This Inner Loop Header: Depth=1
        jmp     LBB0_1
        .cfi_endproc
                                        ## -- End function
As we can see, there is no condition judgement after LBB0_1. Now check this,

LBB0_1:                                 ## =>This Inner Loop Header: Depth=1
        xorl    %eax, %eax
        movb    %al, %cl
        testb   $1, %cl
        jne     LBB0_2
        jmp     LBB0_3
LBB0_2:                                 ##   in Loop: Header=BB0_1 Depth=1
        jmp     LBB0_1
LBB0_3:
        xorl    %eax, %eax
        popq    %rbp
        retq
        .cfi_endproc
LBB0_2 is used to execute next loop, if exit condition meets, jmp LBB0_3 will be executed.

for(;;){}

int main(){

	for(int i = 0; i < 10; i++){
		i += 2;
	}
	return 0;
}

        .section        __TEXT,__text,regular,pure_instructions
        .macosx_version_min 10, 13
        .globl  _main                   ## -- Begin function main
        .p2align        4, 0x90
_main:                                  ## @main
        .cfi_startproc
## BB#0:
        pushq   %rbp
Lcfi0:
        .cfi_def_cfa_offset 16
Lcfi1:
        .cfi_offset %rbp, -16
        movq    %rsp, %rbp
Lcfi2:
        .cfi_def_cfa_register %rbp
        movl    $0, -4(%rbp)
        movl    $0, -8(%rbp)
LBB0_1:                                 ## =>This Inner Loop Header: Depth=1
        cmpl    $10, -8(%rbp)
        jge     LBB0_4
## BB#2:                                ##   in Loop: Header=BB0_1 Depth=1
        movl    -8(%rbp), %eax
        addl    $2, %eax
        movl    %eax, -8(%rbp)
## BB#3:                                ##   in Loop: Header=BB0_1 Depth=1
        movl    -8(%rbp), %eax
        addl    $1, %eax
        movl    %eax, -8(%rbp)
        jmp     LBB0_1
LBB0_4:
        xorl    %eax, %eax
        popq    %rbp
        retq
        .cfi_endproc
                                        ## -- End function
Firstly, i is initialized with 0, then condition judgement is executed in LBB0_1, if condition meets, goto LBB0_4, else BB#2 and BB#3 will be executed.

As wrote before, if condition judgement is not provided, the next step is jmp exit.

label
This is the most basic ways with label, which is common in ASM.