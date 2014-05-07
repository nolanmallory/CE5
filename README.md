CE5
===

##TASK #1: Simple MIPS Assembly Program
Write a MIPS assembly program that will initialize registers $S0 and $S1 with the decimal values 44 and -37 respectively. Use a register instruction to add these values together and store the result in $S2. Finally you will write an instruction that will store $S2 to the hexadecimal memory address x54.
```
addi $s0, $0, 44
addi $s1, $0, -37 
add $s2, $s0, $s1
sw $s2, 0x54($s0)
```

##TASK #2: Machine Code

| Assembly            |    Machine(Binary)           |    Machine(Hex)|
| :----------------: |:-------------------------------:| -----------:|
| addi $s0, $0, 44  | 0000000000000000000000000000000 |  0x2010002C |
| addi $s1, $0, -37 | 0000000000000000000000000000000 |  0x2011FFDB |
| add $s2, $s0, $s1 | 0000000000000000000000000000000 |  0x02119020 |
| sw $s2, 0x54($s0) | 0000000000000000000000000000000 |  0xAC120036 |
###Simulation




##TASK #3: Modified With ORI Instruction
