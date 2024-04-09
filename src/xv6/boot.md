# Xv6启动流程

1. _entry @ kernel/entry.S

   根据hartid将sp指向一个4k的栈，sp = stack0 + (hardid * 4096)，然后跳转到start函数

   ```assembly
   .section .text
   .global	_entry
   _entry:
   	# set up a stack for C.
   	# stack0 is declared in start.c,
   	# with a 4096-byte stack per CPU.
   	# sp = stack0 + (hardid * 4096)
   	la sp, stack0
   	li a0, 1024*4
   	csrr a1, mhartid
   	addi a1, a1, 1
   	mul a0, a0, a1
   	add sp, sp, a0
   	# jump to start() in start.c
   	call start
   	
   spin:
   	j spin
   ```

2. start @ kernel/start.c

   ```c
   void start()
   {
       unsigned long x = r_mstatus();
       x &= ~MSTATUS_MPPP_MASK;
       x |= MSTATUS_MPP_S;
       w_mstatus(x);
       
       
       
   }
   ```

   
