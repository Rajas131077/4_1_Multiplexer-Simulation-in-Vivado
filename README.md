# SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure

1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** and give a name (e.g., `Mux4_to_1`).  
3. Add/create your Verilog files and testbench.  
4. Select an FPGA part (e.g., `xc7a35ticsg324-1L`).  
5. Run **Synthesis** to check for errors.  
6. Run **Simulation** → **Run Behavioral Simulation**.  
7. Observe the waveforms of inputs and outputs.  
8. Adjust simulation time if needed (e.g., 1000ns).  
9. Save the project and take screenshots of results.  
10. Close simulation.  

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation
```verilog

module mux21(a, b, s, y);
 input a, b, s;
 output y;
 wire w1, w2, w3;
 and (w1, a, ~s);
 and (w2, b, s);
 or (y, w1, w2);
endmodule
module MUX41_GL(I, s, y);
 input [3:0] I;
 input [1:0] s;
 output y;
 wire w3, w4;
 mux21 m1 (I[0], I[1], s[0], w3);
 mux21 m2 (I[2], I[3], s[0], w4);
 mux21 m3 (w3, w4, s[1], y);
endmodule

```
### 4:1 MUX Gate-Level Implementation- Testbench
```verilog
`timescale 1ns/1ps
module MUX41_tb;
 reg [3:0] i;
 reg [1:0] s;
 wire y;
 MUX41_GL uut (i, s, y);
 initial begin
 i = 4'b1001;
 s = 2'b00; #10;
 $display("selection %b, output %b", s, y);
 s = 2'b01; #10;
 $display("selection %b, output %b", s, y);
 s = 2'b10; #10;
 $display("selection %b, output %b", s, y);
 s = 2'b11; #10;
 $display("selection %b, output %b", s, y);
 $finish;
 end
endmodule
```
## Simulated Output Gate Level Modelling

<img width="1321" height="743" alt="4 1 mux" src="https://github.com/user-attachments/assets/15fb52b0-c181-4203-b3fd-3d76746b07a4" />


---
### 4:1 MUX Data flow Modelling
```verilog

module MUX41(I,S,y);
input [3:0]I;
input [1:0]S;
output y;
wire [4:1]w;
assign w[1] = I[0] & (~S[1]) & (~S[0]);
assign w[2] = I[1] & (~S[1]) & S[0];
assign w[3] = I[2] & S[1] & (~S[0]);
assign w[4] = I[3] & S[1] & S[0];
assign y = w[1] | w[2] | w[3] | w[4];
endmodule

```
### 4:1 MUX Data flow Modelling- Testbench
```verilog
// Testbench Skeleton
`timescale 1ns/1ps
module MUX_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
MUX41 uut(i,s,y);
initial
begin
i=4'b1001;
s=2'b00;
#10;
$display("selection %b %b, output %b",s[1],s[0],y);
s=2'b01;
$display("selection %b %b, output %b",s[1],s[0],y);
#10;
s=2'b10;
$display("selection %b %b, output %b",s[1],s[0],y);
#10;
s=2'b11;
$display("selection %b %b, output %b",s[1],s[0],y);
#10;
$finish;
end
endmodule


```
## Simulated Output Dataflow Modelling
<img width="1313" height="591" alt="41" src="https://github.com/user-attachments/assets/6a0df207-e095-4d0d-a9fe-cf1df20e1418" />


---
### 4:1 MUX Behavioral Implementation
```verilog
module MUX_4_1(i,s,y);
input [3:0]i;
input [1:0]s;
output reg y;
always @(i,s)
 begin
 case(s)
 2'b00 : y = i[0];
 2'b01 : y = i[1];
 2'b10 : y = i[2];
 2'b11 : y = i[3];
 endcase
 end
endmodule
### 4:1 MUX Behavioral Modelling- Testbench
```verilog
`timescale 1ns/1ps
module MUX_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
MUX_4_1 uut(i,s,y);
initial
begin
i=4'b1001;
s=2'b00;
#10;
$display("selection %b %b, output %b",s[1],s[0],y);
s=2'b01;
$display("selection %b %b, output %b",s[1],s[0],y);
#10;
s=2'b10;
$display("selection %b %b, output %b",s[1],s[0],y);
#10;
s=2'b11;
$display("selection %b %b, output %b",s[1],s[0],y);
#10;
$finish;
end
endmodule

```
## Simulated Output Behavioral Modelling

<img width="1354" height="761" alt="behav" src="https://github.com/user-attachments/assets/6fb30a15-0de5-4853-8c1a-0c6ce358956d" />



### 4:1 MUX Structural Implementation

![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)


```verilog
module MUX21(A,B,S,Z);
input A,B,S;
output Z;
wire W1,W2;
and g1(W1,A,~S);
and g2(W2,B,S);
or g3(Z,W1,W2);
endmodule
module MUX_41(I,S,Y);
input [3:0]I;
input [1:0]S;
output Y;
wire [2:1]W;
MUX21 M1(.Z(W[1]),.A(I[0]),.B(I[1]),.S(S[1]));
MUX21 M2(.Z(W[2]),.A(I[2]),.B(I[3]),.S(S[1]));
MUX21 M3(.Z(Y),.A(W[1]),.B(W[2]),.S(S[0]));
endmodule

```
### Testbench Implementation
```verilog
`timescale 1ns / 1ps
module MUX_41_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
MUX_41 uut(I,S,Y);
initial
begin
I=4'B1000;
S=2'b00;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b01;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b10;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b11;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
$finish;
end
endmodule
## Simulated Output Structural Modelling

<img width="1338" height="705" alt="struct (2)" src="https://github.com/user-attachments/assets/571ede48-6df3-41de-b851-cb7314227d6f" />


---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.

