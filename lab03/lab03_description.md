# Lab03 - Structural verilog for combinational circuits and test bench use (lab03_top)
Important Notes:
1. Name your files as specified in the lab directions for best results.
2. First thing add your name, date, assignment number etc. in the comment block at the beginning.
   This is the first thing that makes it clear it is your code. Also helps with grading.
3. Take screenshots as you go and save them to a properly labeled folder.
4. It may be easier to turn in the verilog as a screen capture also to maintain the legibility and formatting.

# Submission Details
Today's lab will be a pdf report submitted to Canvas. Ideally you will submit your modified verilog, a schematic,
a timing diagram that tests all four gates, and a picture of your board with a person pressing the buttons and
showing the correct output for all gates for one set of inputs. 
Today's lab will be graded as follows:
0. (0 pts) Formatting with team names on top right hand side with title of assignment immediately underneath
   and all right justified. Each picture clearly labeled. Will subtract points if needed.
1. (3 pts) Structural verilog, schematic, and timing diagram with provided test bench for a 2-1 multiplexor.
2. (3 pts) Structural verilog, schematic, and timing diagram with provided test bench for a 1-bit adder.
3. (2 pts) Timing diagram using lab03_top and provided test bench.
4. (2 pts) Two pictures of correct board output a) with SWITCH[0] on and b) with SWITCH[1] off.
   
# Learning outcomes
1. Increasing familiarity with Vivado project creation, simulation, and downloading to the board.
2. Reinforcement of basic circuit design using gates and creating modules to use in larger design.
3. Experience with using a testbench.

# Submission Details
Today's lab will be a pdf report again submitted to Canvas. You will submit 2 designs: a straight forward circuit from a boolean logic expression and a one-bit full adder. Today's lab will be graded as follows: 
1. logic expression design (verilog for combo_eq, circuit screen capture, timing diagram screen capture): 6 pts
2. adder design(verilog for full_adder and full_adder_tb, circuit screen capture, timing diagram screen capture): 4 pts

It is useful to finish these designs as they are a part of later projects and labs. It may be easier to turn in the verilog as a screen capture also to maintain the legibility and formatting.

## Project creation
Start the project
1. Use the `Windows` key to bring up the search bar for `Vivado`.
2. Start `Vivado`.
3. Under Quick Start, choose Create Project.
4. Hit Next to use the assist at creating projects Wizard.
5. Fill in the project name `lab3_combo` and file location.
6. Default is rtl project, which is what you want so just click Next.
7. Don't create a new file yet, just click Next.
8. No constraint file is needed this time, so just click Next again.
9. Select the `Board` tab. Under `Name` find PYNQ-Z1.
10. Select PYNQ-Z1 in table below. Make sure that the Part is xc7z020clg400-1.

# Individual Designs

# Boolean logic design

## Editing file
1. In the middle Sources window choose the plus tab.
2. Choose add or create design sources. Click `Next`.
4. Create new file by choosing the plus tab again, -> Create File. Name it `combo_eq`.
5. In the future you can specify parameters to your Module here, but for now lets make sure 
   to learn the whole structure of a Verilog program so you can write it yourself. Just click OK.
6. Click Yes.
7. Now you will notice in the Sources Window under Design Sources that the `combo_eq` file has been created.
8. Double click on the file and it will open a window on the right side.
9. Fill in the team member names under Engineer. As well as the other pertinent information.

-------
## Write Verilog for x = a'b'+ ab
1. Write structural verilog for the boolean logic expression `x = a'b' + ab`. File name should be `combo_eq`.
2. Create the 'schematic' and verify it is the correct circuit. Recall `RTL analysis` -> `Open Elaborated Design`
3. Edit your verilog if necessary.
4. Once your verilog and circuit are correct get screen captures for your report.
5. In a prior lab, force values were used to create a timing diagram that demonstrates the circuit is functioning correctly. Generating these force values over and over is time consuming and has to be repeated each debugging session. Today we will learn to use a testbench to run a simulation instead.

## Understanding a testbench ##
6. A testbench is a way to run a simulation without typing in tcl commands. Below is one designed to test all inputs of the `combo_eq` module you wrote above. Let's make sure you understand key aspects of the code below.
   
### Timescale
```verilog
`timescale 1 ns/ 1 ns
```
The first unit in the timescale indicates the reference time scale units of delay and the second unit indicates the precision. We will generally just use 1 ns for both.

### Circuit Under Test
Note the name of the module is the same as the CUT (Circuit Under Test) followed by _tb. This is not required, but is convention and highly recommended. Often all signals in the testbench are also followed by _tb, but is omitted here for simplicity. Once the timescale is set the command `#`number can be used to have the simulation delay changes for number time units.
### Registers
`reg a;` or "register a" becomes a variable we can set to hold a signal. Registers are needed for inputs into the instantiation of a module being tested.
### Wires
`wire x;` is literally a wire in a circuit. Wires are needed for outputs from the module being tested.
### Constants
`localparam` is used to define a constant name for more readable and maintainable code. Now when we delay the simulation for a certain number of time units, and want to change it later, we only have to change it in one place.
### Instantiating the circuit under test
`combo_eq combo_eq_tb(a, b, x);` instantiates a combo_eq module "type" that is named combo_eq-tb much like in Java you instantiate an object. In this case instead of passing parameter values to a constructor, you are connecting signals in the test module to the inputs and outputs of the circuit under test.
### Setting signals
Each assignment to a register within the initial block happens in the order given. Though "time" in the simulation does not pass until the `#` directive is used.

**Note the lines before the initial begin are setting up the circuit. Those within the initial clause are for running the simulation.**

```verilog
`timescale 1 ns/ 1 ns

module combo_eq_tb;
    reg a;
    reg b;
    wire x;
          
    localparam time_step = 5;

    combo_eq combo_eq_tb(a, b, x);
    
    initial
        begin   
            a = 0;
            b = 0;
            #time_step;
            
            a = 1;
            b = 0;
            #time_step;                 
            
            a = 1;
            b = 1;
            #time_step;
                                
            a = 0;
            b = 1;
            #time_step;
           
        end
    
endmodule

```
7. Create a new file by using the `+` under Source files and add a simulation source. Copy the testbench code above designed to simulate the verilog module `combo_eq`.
   
## Simulate ##
8. Under the Flow Navigator, use `Run Simulation` to run the code in the testbench. Once you have inspected the timing diagram to verify the correct output for `x` for all possible inputs of `a` and `b`, create a screen capture of the timing diagram for your lab report.

___

# Full Adder
---

## Full adder behavior 
A full adder circuit is the basic fundamental circuit that can perform addition or subtraction. It adds together two binary digits and a `carry_in` bit to produce the `sum` and `carry_out` digit. For example, 8 Full adders can be connected in series to make an 8 bit adder that will add two 8-bit numbers in either unsigned binary or two's complement representation.

A one-bit full adder will take in 3 one-bit inputs: two 1-bit inputs representing the digits to add and a one 1-bit `carry_in`. The full adder will have two outputs: a 1-bit `sum` and 1-bit `carry_out` that when used in conjunction with other one-bit adders will be the `carry_in` to the next more significant bit of the addition. 

## Write Verilog for the one-bit full adder
1. Write structural verilog for the one bit full adder. Name this module `full_adder` and saved to `lab_3`.
2. Create the 'schematic' and verify it is the correct circuit. Recall `RTL analysis` -> `Open Elaborated Design`
3. Edit your verilog if necessary.
4. Once your verilog and circuit are correct get screen captures for your report.
5. Using the template for a testbench shown below, create a testbench for your full_adder and run a simulation to show the adder working for all possible inputs.
   
```verilog
`timescale 1 ns/ 1 ns

module full_adder_tb;
    reg a;
    reg b;
    reg c_in;
    wire sum;
    wire c_out;
          
    localparam time_step = 5;

    full_adder full_adder_tb(a, b, c_in, sum, c_out);
    
    initial
        begin   

// Enter your simulation statements here.
           
        end
    
endmodule
```
6. Run a simulation and create a screen capture of the timing diagram for your lab report.

____



