CE5
===

##TASK #1: Simple MIPS Assembly Program
Write a MIPS assembly program that will initialize registers $S0 and $S1 with the decimal values 44 and -37 respectively. Use a register instruction to add these values together and store the result in $S2. Finally you will write an instruction that will store $S2 to the hexadecimal memory address x54.
```
addi $s0, $0, 44
addi $s1, $0, -37 
add $s2, $s0, $s1
sw $s2, 0x54($0)
```

##TASK #2: Machine Code

| Assembly            |    Machine(Binary)           |    Machine(Hex)|
| :----------------: |:-------------------------------:| -----------:|
| addi $s0, $0, 44  | 0010 0000 0001 0000 0000 0000 0010 1100 |  0x2010002C |
| addi $s1, $0, -37 | 0010 0000 0001 0001 1111 1111 1101 1011 |  0x2011FFDB |
| add $s2, $s0, $s1 | 0000 0010 0001 0001 1001 0000 0010 0000 |  0x02119020 |
| sw $s2, 0x54($0)  | 1010 1100 0001 0010 0000 0000 0011 0110 |  0xAC120036 |
```
0x2010002C     Stores 44 into $s0
0x2011FFDB     Stores -37 into $s1
0x02119020     Adds 44 and -37 and stores them into $s2
0xAC120036     Stores $s2 into the memory address x54
```
###Simulation Task 2
![alt text][logo2]

[logo2]: /Task2_sim.JPG
The simulation above is correct because it shows the values of 44 and -37 being stored into registers 16 and 17 respectively. It then shows the sum of these two in register 18. The output is then set to register 0x54.


##TASK #3: Modified With ORI Instruction
Task 3 required that the new instruction of ori be added to the existing code. This was placed immediately after summing the contents of registers $S1 and $S0. The code added is as follows: 
`ori $S3, $S2, x8000`

When placed in the correct order the complete assembly code looked like the following:
```
addi $s0, $0, 44
addi $s1, $0, -37 
add $s2, $s0, $s1
ori $S3, $S2, x8000
sw $s2, 0x54($0)
```
![alt text][logo21]

[logo21]: /Schematic.JPG
![alt text][logo22]

[logo22]: /ALU_Decoder.JPG
![alt text][logo32]

[logo32]: /Main_Decoder.JPG

###VHDL Alteration
The following code was written to implement the Zero Extend function required for the ORI
```
entity zeroext is -- zero extender
  port(a: in  STD_LOGIC_VECTOR(15 downto 0);
       y: out STD_LOGIC_VECTOR(31 downto 0));
end;
```

```
architecture behave of zeroext is
begin
  y <= X"0000" & a; 
end;
```
This code was written to implement the 4 input MUX that was previously not required. To accomodate the Zero Extend the previous 2 input MUX had to be updated.
```
entity mux4 is -- 4-input multiplexer
  generic(width: integer);
  port(d0, d1, d2, d3: in  STD_LOGIC_VECTOR(width-1 downto 0);
       s:      in  STD_LOGIC_VECTOR(1 downto 0);
       y:      out STD_LOGIC_VECTOR(width-1 downto 0));
end;
```
```
architecture behave of mux4 is
begin
  y <= d0 when (s = "00") else 
				d1 when (s = "01") else
				d2 when (s = "10") else
				d3;
end;
```
The following was changed to allow for the ORI function that was previously unused
`when "001101" => controls <= "1010000011"; --ORI`


###Simulation Task 3
![alt text][logo23]

[logo23]: /Task3_sim.JPG
The wave form is correct because the value of 00008007 is visible in register 19.
