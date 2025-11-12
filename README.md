# 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task

# Aim
To design and simulate a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required
Vivado 2023.1
# Procedure
1. Launch Vivado 2023.1
Open Vivado and create a new project.
2. Design the Verilog Code
Write the Verilog code for the seven-segment display, defining the logic that maps a 4-bit binary input to the corresponding segments (a to g) of the display.
3. Create the Testbench
Write a testbench to simulate the seven-segment display behavior. The testbench should apply various 4-bit input values and monitor the corresponding output.
4. Create the Verilog Files
Create both the design module and the testbench in the Vivado project.
5. Run Simulation
Run the behavioral simulation to verify the output. 
6. Observe the Waveforms
Analyze the output waveforms in the simulation window, and verify that the correct segments light up for each digit.
7. Save and Document Results
Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# Verilog Code
# 4 bit Ripple Adder using Task
```verilog 
module RA (
input [3:0] A, B,
input Cin,
output reg [3:0] Sum,
output reg Cout
);
reg c;          
integer i;
task full_adder;
    input a, b, cin;
    output s, cout;
    begin
        s = a ^ b ^ cin;                  
        cout = (a & b) | (b & cin) | (a & cin); 
    end
endtask
always @(*) begin
    c = Cin; 
    for (i = 0; i < 4; i = i + 1) begin
        full_adder(A[i], B[i], c, Sum[i], c);
    end
    Cout = c; 
end
endmodule
```

# Test Bench
```verilog
`timescale 1ns/1ps
module tb_ripple_adder_task;
reg [3:0] A, B;
reg Cin;
wire [3:0] Sum;
wire Cout;
RA uut (A, B, Cin, Sum, Cout);

initial begin
    $display("Time\tA\tB\tCin\t|\tSum\tCout");
    $monitor("%0t\t%b\t%b\t%b\t|\t%b\t%b", $time, A, B, Cin, Sum, Cout);
    A = 4'b0000; B = 4'b0000; Cin = 0; #10;
    A = 4'b0011; B = 4'b0101; Cin = 0; #10; 
    A = 4'b1111; B = 4'b0001; Cin = 0; #10;  
    A = 4'b1010; B = 4'b0101; Cin = 1; #10;  
    A = 4'b0110; B = 4'b0011; Cin = 0; #10; 
    $finish;
end
endmodule
```

# Output Waveform


<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/0bd296bf-6885-430a-afb9-9cd00076506f" />




# 4 bit Ripple counter using Function
```verilog
module RC (
    input clk, rst,
    output reg [3:0] Q
);
    function [3:0] count;
        input [3:0] x;
        begin
            count = x + 1;   
        end
    endfunction
    always @(posedge clk or posedge rst) begin
    if (rst)
        Q <= 4'b0000;     
    else
        Q <= count(Q);   
    end
endmodule
```
# Test Bench
```verilog
`timescale 1ns/1ps    
module tb_RC_func;
reg clk, rst;      
wire [3:0] Q;      
RC uut (clk,rst,Q);
always #5 clk = ~clk;
initial begin
    $display("Time(ns)\tRST\tQ");
    $monitor("%0t\t%b\t%b", $time, rst, Q);
    clk = 0;
    rst = 1;       
    #10;
    rst = 0;        
    #160;
    rst = 1;
    #10;
    rst = 0;

    #50;
    $finish;    
end
endmodule
```

# Output Waveform 


<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/8fb25390-3b93-4f13-8fcc-82c41a623306" />


# Conclusion
In this experiment, a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task was successfully designed and simulated using Verilog HDL.
