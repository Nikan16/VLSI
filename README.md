# Electronic-Lock

Aim: Build an electronic combination lock with a reset button, two number buttons (0 and 1), and an unlock output. The combination should be 01011 
![image](https://user-images.githubusercontent.com/119850000/226257468-1e3f4d6f-3b78-4620-af26-a80acee0c41a.png)

![image](https://user-images.githubusercontent.com/119850000/226257501-f72f350a-66ce-4f90-ba2c-b3a6b70bf51b.png)


############Design##############   
     
module Elock(input clk,rst,b0,b1,output unlock); parameter  
SR=0,S0=1,S01=2,S010=3,S0101=4,S01011=5;   
reg[2:0]state,next_state;  
always@ (*)  
begin  
if(rst)next_state=SR;  
else  
case(state)  
SR: next_state=b0 ? S0:b1 ? SR: state;  
S0: next_state=b0 ? S0:b1 ? S01: state;  
S01: next_state=b0 ? S010:b1 ? SR: state;  
S010: next_state=b0 ? S0:b1 ? S0101: state;  
S0101: next_state=b0 ? S010:b1 ? S01011: state;  
S01011: next_state=b0 ? S0:b1 ? SR: state;  
default: next_state=SR;  
endcase  
end  
always@ (posedge clk)  
state=next_state;  
assign unlock=state==S01011?1:0;  
endmodule  
  
################TESTBENCH####################  
   
module tb;  
reg clk,b0,b1,rst;   
wire unlock;   
integer i;  
reg [4:0]data;  
always #10clk=~clk;  
Elock l0(clk,rst,b0,b1,unlock);  
initial begin    
$dumpfile("tb.vcd");  
$dumpvars(1,tb);  
$monitor("i=%0d,Time=%0d,rst=%b,b0=%b,b1=%b,unlock=%b",i,$time,clk,rst,b0,b1,unlock);  
clk=0;  
rst=1;  
#50 rst=0;  
data=5'b01011;  
i=0;  
#130 $finish;  
end  
always@ (posedge clk)  
begin  
b0 =data[5-i]?0:1;  
b1 =data[5-i]?1:0;  
i=i+1;  
end  
endmodule  

# SPI-based TEMPERATURE MONITOR

This project will aim at design and immplemention of a SPI-based temperature monitor. The students will design a controller in Verilog to read the data from the sensor (LM70) using the industry-standard SPI protocol, convert the data to a human readable format (deg-C) and drive a set of 7-segment display to display the data. In order to test the Verilog code in realtime application, the Verilog code will be synthesized into a Xilinx's Spartan FPGA board. This will allow the students to test their Verilog code in real time.

Temperature Monitor Block Diagram
![image](https://raw.githubusercontent.com/silicon-vlsi/VLSI-2024/main/docs/tempMonitor-blockDiag-v1-0322.png)

SOME USEFUL LINKS

https://learn.sparkfun.com/tutorials/serial-peripheral-interface-spi
Datasheet: TI SPI-based temperature sensor LM70
Technical Reference: Xilinx Spartan-6 FPGA Development Board
❗ TASKS: ❗

1️⃣ Convert the shift register code to a compact format like this shift_reg <= shifte_reg<<1;
2️⃣ Latch the 8-bit output from the LM70 to outreg[7:0] at the end of read cycle and verify you the value is the same as set in the temperature sensor.
3️⃣ Follow chipverify.com excercises till the Behavioural modeling section. Now you can use iverilog to complete the assignments.
