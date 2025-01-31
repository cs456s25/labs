# Project creation
Start the project
1. Use the Windows key to bring up the search bar for Vivado.
2. Start Vivado.
3. Under Quick Start, choose Create Project.
4. Hit Next to use the assist at creating projects (Wizard).
5. Fill in the project name (lab2_board_demo) and file location.
6. Default is rtl project, which is what you want so just click Next.
7. Don't create a new file yet, just click Next.
8. No constraint file yet, so just click Next again.
9. Select the `Board` tab. Under `Name` find PYNQ-Z1.
10. Select PYNQ-Z1 in table below. Make sure that the Part is xc7z020clg400-1 or it may cause problems later.
11. Choose Finish.

Side note (Spring 2024): The part you chose in lab01 was not the correct part - in fact you may have ended up with some package pin not found style errors. Choosing the Board tab and PYNQ-Z1 should eliminate any of those errors. If later you find yourself with a missing package pin error, you probably did not select the correct part at the beginning. You should be able to change to the correct part by going to `Settings` under the Project Manager, then choosing `General` under Project Settings on the left. On the right under project device you can use the '...' to change the device.

# Editing file
1. In the middle Sources window choose the plus tab.
2. Choose add or create design sources. Click Next.
4. Create new file by choosing the plus tab again, -> Create File. Name it `board_demo'. Click Finish
5. In the future you can specify parameters to your Module here, but for now lets make sure 
   to learn the whole structure of a Verilog program so you can write it yourself. Just click OK.
6. Click Yes.
7. Now you will notice in the Sources Window under Design Sources that the board_demo_top file has been created.
8. Double click on the file and it will open a window on the right side.
9. Fill in the team member names under Engineer. As well as the other pertinent information.
    
Create an initial design by typing in the Verilog below. Note that this module is taking inputs from outside of the fpga. We will learn more about the signals specified today. For now, make sure your spelling and capitalization is exact. Note: this is still structural verilog.

```verilog
// Testing the board LEDS and set up
module board_demo(
    input [1:0] SWITCHES, 
    output [3:0] LEDS
);
    and(LEDS[0], SWITCHES[0], SWITCHES[1]);

endmodule
```

# Viewing the schematic
RTL Analysis -> Open Elaborated Design 

Make sure that your schematic makes sense. 

# Continue creating design
Use the and gate as an example and add an or, xor, and not gate each with the same inputs, but the output should be increasing LEDS. Since the not gate has only one input, use the SWITCHES[0] only.

Again check your schematic. If you are having trouble getting your schematic to update, right-click on Open Elaborated Design and choose Reload.
    
# Simulation
1. `Run simulation` -> `Run behavioral simulation`
2. Use the tcl (the command line) commands or UI (user interface) buttons to run the simulation. tcl commands are listed below to run the simulation.
3. When launching the simulation, all the signals will be in a high impedence state or `Z` and outputs will be `X`.
4. To start the simulation from 0 timestep, type in `restart` in tcl or use the circle arrow icon (looks like a reload page icon) in the top menu on the far right.
5. Force a constant value on an input line (signal)

    1) Select the signal -> right click -> force constant -> force value -> 1
   
   ![force constant](../lab1/rightclick_force_constant.png)

   ![right click](../lab1/rightclick_input_constant.png)

    2) tcl console -> add_force *input name* {value timestep}

Note that to see all of the lines individually, you may need to expand the SWITCHES and LEDS in the waveform window and in the signal window.
       To type into the command line, you will need to type into the dialog box underneath the Tcl Conslol Window.
6. Step into timestep to see the wave form (timing diagram) of the signal
   
   1) The play button with a subscript "T" adds the additional runtime specified in the box immediately to its right. Specify the time to step to next wave
   2) type into tcl console
    ```
        run 10ns
    ```

7. Restart the simulation 
   1) The circle arrow  button will reset and rerun the simulation.
   2) Type into tcl console
   ```verilog 
        restart
   ```

8. Inspecting wave form (timing diagram)
   1) Use the magnifying glass in the User Interface, the three icons to the right of the disk icon in the waveform window.
   2) Apparently zooming in and out is no longer supported on the command line

Note that you can "test" all of the outputs at once as these gates are set up in parallel. Once you are convinced you have everything working correctly, move on to the next section.

# Setting up a constraints file

Download the constraints file at https://github.com/cs456s24/cs456s24/tree/main/lab2 

