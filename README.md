# EXPERIMENT 3: Simulation of All Flip-Flops using Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **blocking assignment (`=`)** inside the `always` block.  
Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Blocking)
```verilog
module SR_FF (
    input  S, 
    input  R,     
    input  clk,   
    input  rst, 
    output reg Q
);
always @(posedge clk) 
begin
if (rst==1)
Q <= 1'b0;  
else 
begin
case ({S,R})
2'b00: Q <= Q;   
2'b01: Q <= 1'b0; 
2'b10: Q <= 1'b1;  
2'b11: Q <= 1'bx; 
endcase
end
end
endmodule
```
### SR Flip-Flop Test bench 
```verilog
module tb_SR_FF;
reg s,r,clk,rst;
wire q;

SR_FF uut (s,r,clk,rst,q);

always #5 clk = ~clk;

initial begin 
    clk = 0; s = 0; r = 0; rst = 1;
    #10 rst = 0;
    #10 s = 1; r = 0;   
    #10 s = 0; r = 0;   
    #10 s = 0; r = 1;   
    #10 s = 1; r = 1;   
    #10 s = 0; r = 0;   
    #20 $finish;
end

initial begin
    $monitor("Time=%0t | clk=%b rst=%b | s=%b r=%b | q=%b", 
              $time, clk, rst, s, r, q);
end

endmodule
```
#### SIMULATION OUTPUT

<img width="1919" height="1077" alt="SR FF" src="https://github.com/user-attachments/assets/6b5e1804-b56f-493a-8755-53778c94959a" />
---

### JK Flip-Flop (Blocking)
```verilog
timescale 1ns / 1ps
module JK_FF(j,k,clk,rst,q);
input j,k,clk, rst;
output reg q;
always @(posedge clk)
begin 
if (rst==1)
q=1'b0;
else 
begin 
case ({j,k})
2'b00: q=q;
2'b01: q=1'b0;
2'b10: q=1'b1;
2'b11: q=~q;
endcase
end
end
endmodule

```
### JK Flip-Flop Test bench 
```verilog
module tb_JK_FF;
reg j, k, clk, rst;
wire q;
JK_FF uut (j,k,clk, rst, q);
always #5 clk =~clk;
initial
begin
 clk = 0; j = 0; k = 0; rst = 1;
    #10 rst = 0;
    #10 j = 1; k = 0;   
    #10 j = 0; k = 0;   
    #10 j = 0; k = 1;   
    #10 j = 1; k = 1;   
    #10 j = 0; k = 0;   
    #20 $finish;
end

initial begin
    $monitor("Time=%0t | clk=%b rst=%b | j=%b k=%b | q=%b", 
              $time, clk, rst, j, k, q);
end

endmodule



```
#### SIMULATION OUTPUT

<img width="1919" height="1079" alt="JK_FlipFlop" src="https://github.com/user-attachments/assets/18ea9d46-c855-4f10-819d-0dc4e5476a76" />
---

### D Flip-Flop (Blocking)
```verilog
`timescale 1ns / 1ps
module D_FF(
clk, rst, d, q
    );
    input clk, rst, d;
    output reg q;
    always @ (posedge clk)
begin 
  if(rst==1)
    q=1'b0;
  else
    q=q;
 end
endmodule
```
### D Flip-Flop Test bench 
```verilog
 module tb_D_FF;
reg clk, rst, d;
wire q;
D_FF uut (clk, rst, d, q);
always #5 clk = ~clk;
initial 
begin 
clk = 0;
d = 0;
rst = 1;
#10 rst = 0;
d=0;
#10 d = 1;
end
endmodule


```

#### SIMULATION OUTPUT

<img width="1919" height="1079" alt="D_FlipFlop" src="https://github.com/user-attachments/assets/46b43421-ff38-44dd-8b51-191ef78671f0" />
---
### T Flip-Flop (Blocking)
```verilog
`timescale 1ns / 1ps
module T_FF(
clk, rst, t, q
    );
    input clk, rst, t;
    output reg q;
    always @ (posedge  clk)
    begin 
    if (rst==1)
    q=1'b0;
    else if(t==0)
    q=q;
    else
    q=~q;
    end
endmodule
```
### T Flip-Flop Test bench 
```verilog
module tb_T_FF;
reg clk, rst, t;
wire q;
T_FF uut (clk, rst, t, q);
always #5 clk = ~clk;
initial begin 
clk = 0; t = 0; rst = 1;
#10 rst = 0;
t = 0;
#10 t =1;
end
endmodule


```

#### SIMULATION OUTPUT

<img width="1919" height="1078" alt="T_FlipFlop" src="https://github.com/user-attachments/assets/11bad9cd-0b4e-40b6-96b6-065179cce893" />

---

### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.
