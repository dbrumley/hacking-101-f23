Simplest makefile

```makefile
main: main.o
```

Adding in a new rule:
```makefile
main: main.o

clean:
  rm -f main
```

```C
cur_location = COMPILE_TIME_RANDOM;
shared_mem[cur_location ^ prev_location]++; 
prev_location = cur_location >> 1;
```

Example reverse engineering on Rey

```asm
```asm
4486 <enc>
4486:  0b12           push	r11
4488:  0a12           push	r10
448a:  0912           push	r9
448c:  0812           push	r8
448e:  0d43           clr	r13
4490:  cd4d 7c24      mov.b	r13, 0x247c(r13)		# Move i into address 0x247c+i
4494:  1d53           inc	r13
4496:  3d90 0001      cmp	#0x100, r13				# Loop until r13 is 256
449a:  fa23           jne	#0x4490 <enc+0xa>		# Loop back if not repeated 256 times

449c:  3c40 7c24      mov	#0x247c, r12			# r12 = 0x247c
44a0:  0d43           clr	r13						# r13 = 0
44a2:  0b4d           mov	r13, r11				# i = 0

													# Label 1
44a4:  684c           mov.b	@r12, r8				# r8 = (0x247c+i)
44a6:  4a48           mov.b	r8, r10					# r10 = (0x247c+i)
44a8:  0d5a           add	r10, r13				# r13 = r10+r13
44aa:  0a4b           mov	r11, r10				# r10 = i
44ac:  3af0 0f00      and	#0xf, r10				# r10 = i & 0xf
44b0:  5a4a 7244      mov.b	0x4472(r10), r10		# r10 = (0x4472+ i & 0xf)
44b4:  8a11           sxt	r10
44b6:  0d5a           add	r10, r13				# r13 = r13 + r10
44b8:  3df0 ff00      and	#0xff, r13				# 
44bc:  0a4d           mov	r13, r10
44be:  3a50 7c24      add	#0x247c, r10
44c2:  694a           mov.b	@r10, r9
44c4:  ca48 0000      mov.b	r8, 0x0(r10)			# ()
44c8:  cc49 0000      mov.b	r9, 0x0(r12)			# (0x247c+i) = r9
44cc:  1b53           inc	r11						# i++
44ce:  1c53           inc	r12						# r12 = 0x247c+i
44d0:  3b90 0001      cmp	#0x100, r11
44d4:  e723           jne	#0x44a4 <enc+0x1e>		# Jump for a loop (back to Label 1 if r11 not 256)

44d6:  0b43           clr	r11
44d8:  0c4b           mov	r11, r12
44da:  183c           jmp	#0x450c <enc+0x86>		# Jump to label 2

44dc:  1c53           inc	r12						# Label 3
44de:  3cf0 ff00      and	#0xff, r12
44e2:  0a4c           mov	r12, r10
44e4:  3a50 7c24      add	#0x247c, r10
44e8:  684a           mov.b	@r10, r8
44ea:  4b58           add.b	r8, r11
44ec:  4b4b           mov.b	r11, r11
44ee:  0d4b           mov	r11, r13
44f0:  3d50 7c24      add	#0x247c, r13
44f4:  694d           mov.b	@r13, r9
44f6:  cd48 0000      mov.b	r8, 0x0(r13)
44fa:  ca49 0000      mov.b	r9, 0x0(r10)
44fe:  695d           add.b	@r13, r9
4500:  4d49           mov.b	r9, r13
4502:  dfed 7c24 0000 xor.b	0x247c(r13), 0x0(r15)	# STORES SOME KIND OF RESULT INTO 0x2400+i
4508:  1f53           inc	r15
450a:  3e53           add	#-0x1, r14

450c:  0e93           tst	r14						# Label 2
450e:  e623           jnz	#0x44dc <enc+0x56>		# If r14 not zero, jump to Label 3
4510:  3841           pop	r8
4512:  3941           pop	r9
4514:  3a41           pop	r10
4516:  3b41           pop	r11
4518:  3041           ret
```
```