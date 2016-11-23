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

## Programming Functionality

### If Statements

```c
if (A>=B) {
	cmd_1;
	cmd_2;
}
cmd_3;
```

```Assembler
1. LOAD R1 30 ; ‘A’
2. LOAD R2 31 ; ‘B’
3. SUB R0 R1 R2
4. BNEG 7
5. cmd_1
6. cmd_2 7. cmd_3
```

### While Statements

```
while (A+1!=0) {
	cmd_1;
	cmd_2;
}
cmd_3;
```
```
1. LOAD R1 30 	; ‘A’
2. LOAD R2 31 	; constant 1
3. ADD R3 R1 R2
4. BZERO 8
5. cmd_1
6. cmd_2
7. BRANCH 3
8. cmd_3
```

###For Loop
```
for (A=0; A<=10; ++A){
	cmd_1;
	cmd_2;
}
cmd_3;
```
```
1. SUB R1 R1 R1 ; ‘A’
3. LOAD R2 31 ; constant 10
4. SUB R1 R2 R0
5. BNEG 12
6. cmd_1
7. cmd_2
8. ADDI R1 1 9. BRANCH 4 12.cmd_3
```


## Architecture
### Processor:
* Control:
	* Instruction Memory
		* Stores the program compiled onto it
	* Program Counter (PC)
* Datapath:
	* ALU
	* Register(s)
	* MUX(s)

### Instruction set
* Defines what the processor is capable of.
* Depends on available processor modules
	* (can depend on instruction set)

##Running Gezel
Use the commando:```fdlsim code.fdl <cycles>```where code.fdl is the filename and <cycles> is how many clock cycles you want it to run for.

## 2.4

### Todo:
* Need to fix instruction set (expand with Hjort stuff).

## Processor
>processor <- master -> bus <- slave -> interfaces

* There are three slaves
* All slaves are connected to `M_bus_cmd`
* Each slave has a targetID
* Processor sends **data** and **commands** to bus
* Bus passes

The first 4 bits of the M_cmd(32 bits) determine which slave the data and cmd goes to.

Based on the S_bus_cmd (which for all slave 2q is connected to M_bus_cmd)

A particular slave interface reacts on this request by setting S_bus_ack to ’1’ and sends the command to the slave unit.


The slave responds to this data.
This is defined by the targetID signal, which is defined when the slave bus interface is initialized and will never change value.
The figure also shows the targetID value for each of the 3 slaves in the system.

* Connect processor to rest of embedded system
	* connect processor to bus (*platform.fdl*)
		* processor should be connected to master bus interface, handling the communications protocol.

### Communications protocol

* processor communicates two types of Data:
	* `cmd`: commands that processor wants slave to execute
	* `data`: data related to the command
* communication procedure:
	1. Processor sets:
		* `data -> M_datanin`
		* `cmd -> M_cmd`
		* `M_datainrdy` to 1.
	2. Master interface detects assertion of `M_datainrdy` and transfers cmd and data to bus and sets:
		* `M_cmd -> M_bus_cmd`  
		* `M_datain -> M_bus_dataout`
		* `M_bus_req` to 1
	3. The `S_bus_cmd` port on each slave interface is connected to `M_bus_cmd` on the master interface. When a particular slave is identified, it sets `S_bus_ack` to 1. Thereafter the slave interface passes the command to that particular slave unit.

`M_cmd(32) : targetID(4) + command(28)`

> on NOP assert storenable = 0
> on brsel, either reserve adress bits exclusively for brsel
>... or share with asel and dissable storenable

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

## Links
[DEC/BIN/HEX Converter](https://conv.darkbyte.ru)
[Two's complement converter](http://www.exploringbinary.com/twos-complement-converter/)
[BitLength Converter](https://mothereff.in/byte-counter)
[Hex to Bin](http://www.binaryhexconverter.com/hex-to-binary-converter)
[Bin to Hex](http://www.binaryhexconverter.com/binary-to-hex-converter)


## Report Structure
* Front cover
	- Group: Name, Student No, Signature
	- List of who did what, implementation and report
* Design:
	- Description of selected instruction as well as a table or figure of the complete instruction set.
	- Reasoning for why these instructions are chosen for the realization of the CPU.
	- Don't just describe what you have done, also describe why.
* Implementation
	- A diagram of the full implementation of the CPU.
	- A description of
Et diagram over den fulde implementering af jeres CPU
En beskrivelse af hvordan I har implementeret instruktionerne og en begrundelse for hvorfor de er implementeret på den måde. I kan nøjes med at beskrive de væsentligste instruktioner, dvs. dem der stiller særlige krav og dem som I mener at have fundet en særlig elegant løsning til.
Specielt er det vigtigt at der bliver redegjort for konsekvenser af at have ændret på den basale algoritme for at opnå en mere smidig løsning, f.eks. som at lave division ved bit-shift.
3. Resultater

Performance analyse
Dokumenter tidsforbruget, f.eks. i form af antal clock cycles der bruges til at udregne et datapunkt.
Prøv at finde ud af skifteaktiviteten 0->1 og 1->0 for udregningen af et datapunkt - det vil give en indikation om energiforbrug, MEN er på ingen måde eksakt!
Husk at reflekter over det I kommer frem til - giver det mening, er det som forventet?
4. Konklusion

Kort konklusion på opgaven. Hvad kunne betale sig at lave som en dedikeret hardware løsning?



5. Referencer

Her skal listes de referencer som i har brugt - det kan være artikler eller hjemmesider hvor I har fundet inspiration og forståelse.
