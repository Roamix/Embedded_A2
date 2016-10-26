# Embedded Systems
## MIPS CPU Assembler Code
If C is the high level code, a compiler compiles to MIPS CPU Assembler code. The MIPS CPU Code is assembled into machine code.

### Knob & Switch:
[Knob & Switch instruction set](http://users.dickinson.edu/~braught/talksnpapers/ccscne01/KandS/instructions.html)

[Virtualizer](http://users.dickinson.edu/~braught/kands/KandS2/machine.html)

Components: Actions `LOAD, STORE, MOVE...`, Data types: Memory Adresses `1,2,...[M]` & Registers `R0, R1, R2...[R]`, Operators: `ADD, SUB, AND, OR...`

An instruction combines these components to form commands which the machine follows one at the time.

Memory addresses can hold instructions or vales `LOAD R1 12`, `ADD R1 R1 2` and `3` are all valid values for memory addresses.

Instruction					| Meaning
---								| ---
**Data Movement**			|
`LOAD [R] [M]`				| Load content of `M` into `R`
`STORE [M] [R]`				| Store content of `R` into `M`
`MOVE [R1] [R2]`				| Move (duplicate) content of `R` to `R`
**Arithmetic and Logic**	| *`R1 = R2 OP R3`*
``ADD [R1] [R2] [R3]``		| Addition operator `OP is +`
``SUB [R1] [R2] [R3]``		| Subtraction operator `OP is -`
``AND [R1] [R2] [R3]``		| Or operator `OP is v`
``OR [R1] [R2] [R3]``		| And operator`OP is ^`
**Branching**					|
`BRANCH [M]`					| Jump to line M
`BZERO [M]`					| If ALU results is zero
`BNEG [M]`					| If ALU results is negative
**Other Instructions**		|
`NOP`							| Do nothing (i.e stall)
`HALT`							| Stop the machine

***
Converting assembly code to binary:
```
R-type: [OP][R1][R2][R3]
I-type: [OP][R1][I]
```
***

##Programming Functionality

###If Statements

```c
if (A>=B) {	cmd_1;
	cmd_2;
}cmd_3;
```
```
1. LOAD R1 30 ; ‘A’
2. LOAD R2 31 ; ‘B’
3. SUB R0 R1 R2
4. BNEG 75. cmd_1
6. cmd_2 7. cmd_3
```

###While Statements

```
while (A+1!=0) {	cmd_1;	cmd_2;
}cmd_3;
```
```
1. LOAD R1 30 	; ‘A’
2. LOAD R2 31 	; constant 1
3. ADD R3 R1 R2
4. BZERO 85. cmd_1
6. cmd_2
7. BRANCH 3
8. cmd_3
```

###For Loop
```
for (A=0; A<=10; ++A){
	cmd_1;
	cmd_2;
}cmd_3;
```
```
1. SUB R1 R1 R1 ; ‘A’3. LOAD R2 31 ; constant 10
4. SUB R1 R2 R0
5. BNEG 126. cmd_17. cmd_28. ADDI R1 1 9. BRANCH 4 12.cmd_3
```
##Architecture
###Processor:
* Control:
	* Instruction Memory
		* Stores the program compiled onto it
	* Program Counter (PC)
* Datapath:
	* ALU
	* Register(s)
	* MUX(s)

###(Instruction set
* Defines what the processor is capable of.
* Depends on available processor modules
	* (can depend on instruction set)

##Running Gezel
Use the commando:```fdlsim code.fdl <cycles>```where code.fdl is the filename and <cycles> is how many clock cycles you want it to run for.

## Repository Maintenance

==Remember to CD to directory!==

###Push
```
git add .
git commit -m "comment"
git push -u origin master
```
####1 liner:
```
git add . ; git commit -m "Message to the main"; git push -u origin master
```

###Pull
```
git pull origin master
```
