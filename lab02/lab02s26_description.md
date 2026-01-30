# Lab02 - Simulation review and Board testing (lab02_board_demo)
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
1. (2 pts) Formatting with team names on top right hand side with title of assignment
    immediately underneath and all right justified. Each picture clearly labeled.
3. (2 pts) Board picture.
4. (2 pt) Verilog with header and names for all the gates connected to the board.
5. (2 pt) Schematic corresponding to the verilog for all gates connected to board inputs and outputs.
6. (2 pt) Timing diagram of testing the circuit before downloading on the board.
   
# Learning outcomes
1. Increasing familiarity with Vivado project creation, simulation, and downloading to the board.
2. Reinforcement of basic gate logic and timing diagrams.
3. Demonstration of exercising circuit on the board.

# Project creation
Start the project
1. Use the Windows key to bring up the search bar for Vivado.
2. Start Vivado.
3. Under Quick Start, choose Create Project.
4. Hit Next to use the assist at creating projects (Wizard).
5. Fill in the project name (lab02_board_demo) and file location.
6. Default is rtl project, which is what you want so just click Next.
7. Select the `Board` tab. Under `Name` find PYNQ-Z1.
8. Select PYNQ-Z1 in table below. Make sure that the Part is xc7z020clg400-1 or it may cause problems later.
    Specifically, not being able to find certain inputs and outputs(a missing package pin error)
    when translated to the board. You should be able to change to the correct part by going to `Settings`
    under the Project Manager, then choosing `General` under Project Settings on the left. On the right under
    project device you can use the '...' to change the device.
9. Choose Next, then Finish.

# Editing file
1. In the middle Sources window choose the plus tab.
2. Choose add or create design sources. Click Next.
4. Create new file by choosing the plus tab again, -> Create File. Name it `board_demo'. Click OK, then Finish.
5. In the future you can specify parameters to your Module here, but for now lets make sure 
   to learn the whole structure of a Verilog program so you can write it yourself. Just click OK.
6. Click Yes.
7. Now you will notice in the Sources Window under Design Sources that the board_demo.v file has been created.
8. Double click on the file and it will open a window on the right side.
9. Fill in the team member names under Engineer. As well as the other pertinent information.
    
Create an initial design by typing in the Verilog below. Note that this module is taking inputs from outside of the
fpga. We will learn more about the signals specified today. For now, make sure your spelling and capitalization is
exact. Note: this is still structural verilog.

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
RTL Analysis -> Open Elaborated Design -> OK

Make sure that your schematic makes sense. 

# Continue creating design
Use the and gate as an example and add an or, xor, and not gate each with the same inputs, but the output should be
increasing LEDS. Since the not gate has only one input, use the SWITCHES[0] only.

Again check your schematic. If you are having trouble getting your schematic to update, right-click on 
Open Elaborated Design and choose Reload.
    
# Simulation
1. `Run simulation` -> `Run behavioral simulation`
2. Use the tcl (the command line) commands or UI (user interface) buttons to run the simulation. tcl commands are
    listed below to run the simulation.
3. When launching the simulation, all the signals will be in a high impedence state or `Z` and outputs will be `X`.
4. To start the simulation from 0 timestep, type in `restart` in tcl or use the circle arrow icon (looks like a reload
    page icon) in the very top menu on the far right.
5. Force a constant value on an input line (signal)

    a) Select the signal -> right click -> force constant -> force value -> 1
   
   ![force constant](../lab01/rightclick_force_constant.png)

   ![right click](../lab01/rightclick_input_constant.png)

    b) tcl console -> add_force *input name* {value timestep}

Note that to see all of the lines individually, you may need to expand the SWITCHES and LEDS in the waveform window
and in the signal window. To type into the command line, you will need to type into the dialog box underneath the 
Tcl Console Window.

6. "Step into timestep" to see the wave form (timing diagram) of the signal
   a) The play button with a subscript "T" adds the additional runtime specified in the box immediately to its right.
   Specify the time to run, or
   b) type into tcl console
    ```
        run 10ns
    ```

7. Restart the simulation 
   a) The circle arrow  button will reset and rerun the simulation.
   b) Type into tcl console
   ```verilog 
        restart
   ```

8. Inspecting wave form (timing diagram)
   a) Use the magnifying glass in the User Interface, the three icons to the right of the disk icon in the waveform window.
   b) Apparently zooming in and out is no longer supported on the command line

Note that you can "test" all of the outputs at once as these gates are set up in parallel. Once you are convinced you have everything working correctly, move on to the next section.

### Take screenshots as you go so you can upload them later.
This lab is purposefully a bit more open-ended and practice. Do the best you can.

### Setting up a constraints file
Below is a link to the constraints file. 
Download the constraints file at https://github.com/cs456s25/labs/blob/main/lab02/PYNQ-Z1_C.xdc 
To add the constraints file go to the Sources tab -> Constraints -> constrs_1 then choose +
Add or create constraints should be selected. Click Next.
Click on Add Files. Then navigate to where you downloaded the file from above and select it.
Choose OK and then Finish.

Go back to the Sources tab and open the PYNQ-Z1_C.xdc file. You will need to edit it.
Uncomment a total of six line. The two with SWITCHES in them and then the four corresponding to LEDS[0-3]. Save.

###
Get the equipment suitcase from a locker. See the instructor for a locker number and combination. 
You will only need the container with the pink stryrofoam and the PYNQ board. Leave the other cables alone.

### Programming the board
1. Plug in the board by using the USB cord and plugging the USB side into any USB port on the back of the computer.
    Then slide the other end into the appropriate connector on the PYNQ board, right above the words PROG UART.
   Power on the PYNQ board with the power switch on the bottom left. A red light should come on.
3. Choose generate bitstream from under the Program and Debug menu on the left. Accept the defaults and click OK.
   This takes some time. You can see the system working by the spinning green circle in the top right. Just wait.
   Click OK.
5. Open Target. You should now be able to choose Open Target under the Open Hardware Menu on the left.
   Choose Open Target and Auto Connect. After a bit you should be able to choose Program Device.
7. Program Device. When you click on Program Device, the part xc7z020_1 will pop up. Choose it. Accept defaults and click Program.
8. The LEDS should now be lit up for a one output and not lit for a zero output based on the values of the SWITCHES.
Take a picture or two of the board working with a variety of switch combinations. Try to get all four possibilities.

Once you are done, power off the board before disconnecting it and putting it away.



