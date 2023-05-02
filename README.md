Download Link: https://assignmentchef.com/product/solved-cop4530-pp3-gritvm-interpreter
<br>
For Programming Project 3, you will be implementing an interpreter for a programming language called GritVM. Your GritVM will read in a file of code written in this specific GVM language, run the instructions, and be able to return the results held in the GritVM object’s memory. The GritVM has two variables that contain its memory: a NodeList that contains the instructions and a Vector that contains the data. There will also be a single variable called the accumulator. The accumulator stores the results of various operations as an intermediate. The accumulator is an implicit operand for all mathematical calculations.

Below is an example of some of your private members for the GritVM.

std::vector&lt;long&gt; dataMem; std::list&lt;Instruction&gt; instructMem;

std::list&lt;Instruction&gt;::iterator currentInstruct; STATUS machineStatus; long accumulator;

For this project. I am allowing you to use the C++ Standard Template Library version of the Vector and List. You must use these STL data types for your GritVM. The GritVM can only operate on long data types, so that is the only data type you will need to worry about for your machine data.

You will be responsible for programming in the functionality of the following instructions in your implementation of GritVM. Notice that the memory management functions match the Vector ADT nearly identically. DM represents data memory:




<table width="624">

 <tbody>

  <tr>

   <td width="98"><strong>Instruction</strong></td>

   <td width="156"><strong>Type</strong></td>

   <td width="106"><strong>Call</strong></td>

   <td width="263"><strong>Notes</strong></td>

  </tr>

  <tr>

   <td width="98"><strong>CLEAR</strong></td>

   <td width="156">Accumulator Functions</td>

   <td width="106">CLEAR</td>

   <td width="263">Set the accumulator to 0</td>

  </tr>

  <tr>

   <td width="98"><strong>AT</strong></td>

   <td width="156">DM Management Functions</td>

   <td width="106">AT X</td>

   <td width="263">Sets the accumulator to the value atDM[X]A = DM[X]</td>

  </tr>

  <tr>

   <td width="98"><strong>SET</strong></td>

   <td width="156">DM Management Functions</td>

   <td width="106">SET X</td>

   <td width="263">Sets the DM[X] to the value in the accumulator DM[X] = A</td>

  </tr>

  <tr>

   <td width="98"><strong>INSERT</strong></td>

   <td width="156">DM Management Functions</td>

   <td width="106">INSERT X</td>

   <td width="263">Inserts in DM at location X the value in the accumulator</td>

  </tr>

  <tr>

   <td width="98"><strong>Instruction</strong></td>

   <td width="156"><strong>Type</strong></td>

   <td width="106"><strong>Call</strong></td>

   <td width="263"><strong>Notes</strong></td>

  </tr>

  <tr>

   <td width="98"><strong>ERASE</strong></td>

   <td width="156">DM Management Functions</td>

   <td width="106">ERASE X</td>

   <td width="263">Erases location X in the DM</td>

  </tr>

  <tr>

   <td width="98"><strong>ADDCONST</strong></td>

   <td width="156">Accumulator Maths with a Constant</td>

   <td width="106">ADDCONST C</td>

   <td width="263">Adds C to the accumulator value A = A + C</td>

  </tr>

  <tr>

   <td width="98"><strong>SUBCONST</strong></td>

   <td width="156">Accumulator Maths with a Constant</td>

   <td width="106">SUBCONST C</td>

   <td width="263">Subtracts C from the accumulator A = A – C</td>

  </tr>

  <tr>

   <td width="98"><strong>MULCONST</strong></td>

   <td width="156">Accumulator Maths with a Constant</td>

   <td width="106">MULCONST C</td>

   <td width="263">Multiplies C to the accumulator value A = A * C</td>

  </tr>

  <tr>

   <td width="98"><strong>DIVCONST</strong></td>

   <td width="156">Accumulator Maths with a Constant</td>

   <td width="106">DIVCONST C</td>

   <td width="263">Divides C from the accumulator value A = A / C</td>

  </tr>

  <tr>

   <td width="98"><strong>ADDMEM</strong></td>

   <td width="156">Accumulator Maths with a Memory Location</td>

   <td width="106">ADDMEM X</td>

   <td width="263">Adds DM[X] to the accumulator value A = A + DM[X]</td>

  </tr>

  <tr>

   <td width="98"><strong>SUBMEM</strong></td>

   <td width="156">Accumulator Maths with a Memory Location</td>

   <td width="106">SUBMEM X</td>

   <td width="263">Subtracts DM[X] from the accumulator A = A – DM[X]</td>

  </tr>

  <tr>

   <td width="98"><strong>MULMEM</strong></td>

   <td width="156">Accumulator Maths with a Memory Location</td>

   <td width="106">MULMEM X</td>

   <td width="263">Multiplies DM[X] to the accumulator value A = A * DM[X]</td>

  </tr>

  <tr>

   <td width="98"><strong>DIVMEM</strong></td>

   <td width="156">Accumulator Maths with a Memory Location</td>

   <td width="106">DIVMEM X</td>

   <td width="263">Divides DM[X] from the accumulator value A = A / DM[X]</td>

  </tr>

  <tr>

   <td width="98"><strong>JUMPREL</strong></td>

   <td width="156">Instruction Jump Functions</td>

   <td width="106">JUMPREL Y</td>

   <td width="263">Goes forward/back Y instructions from the current instruction (can be negative)</td>

  </tr>

  <tr>

   <td width="98"><strong>JUMPZERO</strong></td>

   <td width="156">Instruction Jump Functions</td>

   <td width="106">JUMPZERO Y</td>

   <td width="263">Goes forward/back Y instructions from the current instruction (can be negative) if accumulator is 0. Otherwise just move forward 1 from the current instruction.</td>

  </tr>

  <tr>

   <td width="98"><strong>JUMPNZERO</strong></td>

   <td width="156">Instruction Jump Functions</td>

   <td width="106">JUMPNZERO Y</td>

   <td width="263">Goes forward/back Y instructions from the current instruction (can be negative) if accumulator is not 0. Otherwise just move forward 1 from the current instruction.</td>

  </tr>

  <tr>

   <td width="98"><strong>NOOP</strong></td>

   <td width="156">Misc Functions</td>

   <td width="106">NOOP</td>

   <td width="263">Perform no operation. Counts as an instruction but does nothing.</td>

  </tr>

  <tr>

   <td width="98"><strong>HALT</strong></td>

   <td width="156">Misc Functions</td>

   <td width="106">HALT</td>

   <td width="263">Stop the GritVM and switch status toHALTED</td>

  </tr>

  <tr>

   <td width="98"><strong>OUTPUT</strong></td>

   <td width="156">Misc Functions</td>

   <td width="106">OUTPUT</td>

   <td width="263">Output accumulator to std::cout</td>

  </tr>

  <tr>

   <td width="98"><strong>CHECKMEM</strong></td>

   <td width="156">Misc Functions</td>

   <td width="106">CHECKMEM Z</td>

   <td width="263">Checks to make sure DM is of at least size Z. If not, switch status to ERRORED</td>

  </tr>

 </tbody>

</table>

The GritVM object can be in multiple states during its object lifetime. The table below shows  these statuses:

<table width="624">

 <tbody>

  <tr>

   <td width="287"><strong>Status</strong></td>

   <td width="337"><strong>Meaning</strong></td>

  </tr>

  <tr>

   <td width="287"><strong>WAITING</strong></td>

   <td width="337">Waiting to load a program into instruction memory</td>

  </tr>

  <tr>

   <td width="287"><strong>READY</strong></td>

   <td width="337">A program has been loaded into instruction memory and is ready to run</td>

  </tr>

  <tr>

   <td width="287"><strong>RUNNING</strong></td>

   <td width="337">The machine is actively running a program</td>

  </tr>

  <tr>

   <td width="287"><strong>HALTED</strong></td>

   <td width="337">The program halted the machine whether by a HALT instruction or reaching the end of the instructions</td>

  </tr>

  <tr>

   <td width="287"><strong>ERRORED</strong></td>

   <td width="337">The machine stopped because of an error in the program</td>

  </tr>

  <tr>

   <td width="287"><strong>UNKNOWN</strong></td>

   <td width="337">Unknown status. Should never happen in normal control flow.</td>

  </tr>

 </tbody>

</table>

The following image demonstrates how the GritVM moves between states.

The primary flow of the machine should be as follows:

<ul>

 <li>When created, the GritVM sets the accumulator to 0, and it’s status to WAITING</li>

 <li>A program is loaded into the GritVM object by filename (by the load method)</li>

 <li>If the machine status is anything other than WAITING, return the current status</li>

 <li>If the GritVM cannot read a file, the method throws an exception</li>

 <li>Each line of the file is read in and parsed into its proper instruction and argument</li>

 <li>If the instruction is not recognized, change the machine status to ERRORED and return</li>

 <li>If the line is blank or starts with a #, then skip that line (it is a comment or white space) • Add that instruction to the instructMem list</li>

 <li>If the instructMem size is 0 set the status to WAITING,</li>

 <li>Otherwise, set it to READY</li>

 <li>Copy the vector passed into the load method to the dataMem vector</li>

 <li>Return the current status</li>

 <li>If a GritVM object is READY receiving a call to the run method, then the instructions can be evaluated</li>

 <li>Otherwise, return the current status</li>

 <li>Evaluate the current instruction</li>

 <li>After evaluation, move the current instruction forward the proper amount</li>

 <li>1 for regular instructions</li>

 <li>Y for jumps if a jump is triggered (where Y != 0, if Y is 0, set status to ERRORED and return run method)</li>

 <li>Set status to HALTED if the last instruction was HALT or the last instruction has been reached</li>

 <li>Return the current status</li>

</ul>

<strong>Abstract Class Methods </strong>

There will be an abstract class provided for your GritVM class to inherit from. The following methods must be defined:

<strong>STATUS load(const std::string filename, const std::vector&lt;long&gt; &amp;initialMemory) </strong>

This method loads the GVM program into the GritVM object and copies initialMemory into the data memory. Returns the current machine status. Throws if file cannot be read.

<strong>STATUS run()  </strong>

This method starts the evaluation of the instructions. Returns the current machine status.

<strong>std::vector&lt;long&gt; getDataMem() </strong>

Returns a copy of the current dataMem

<strong>STATUS reset() </strong>

Sets the accumulator to 0, clears the dataMem and instructMem, sets the machineStatus to WAITING.

<strong>Other things in GritVMBase hpp/cpp </strong>

Also provided in the base class files are the enums that define the various stats and instructions for the GritVM. There is an instruction struct for holding an instruction, and it’s argument. There are also five helper methods for you. All of these methods are in the namespace GVMHelper.

<strong>std::string statusToString(STATUS s); </strong>

Converts a status value to a string version of that status.

<strong>STATUS stringToStatus(std::string s); </strong>

Converts a string of a status value to an enum of that status.

<strong>std::string instructionToString(INSTRUCTION_SET s); </strong>

Converts an instruction value to a string version of that instruction.

<strong>INSTRUCTION_SET stringtoInstruction(std::string s); </strong>

Converts a string of an instruction value to an enum of that instruction.

<strong>Instruction parseInstruction(std::string GVMLine); </strong>

Given a line of a GVM file, this function returns an Instruction struct containing the instruction enum and the long argument (if applicable)

<strong>Examples </strong>

Below are some examples of how your code should run

<strong>GritVM vm; </strong>

<strong>// Status: WAITING </strong>

<h1><a name="_Toc12429"></a>// Accumulator: 0</h1>

<strong>// *** Data Memory *** </strong>

<strong>// *** Instruction Memory *** </strong>

<strong>vm.load(“altseq.GVM”, inMem); </strong>

<strong>// Status: READY </strong>

<strong>// Accumulator: 0 </strong>

<strong>// *** Data Memory *** </strong>

<strong>// Location 0: 15 </strong>

<strong>// *** Instruction Memory *** </strong>

<strong>// Instruction 0: CHECKMEM 1 // Instruction 1: CLEAR 0 </strong>

<strong>// Instruction 2: ADDCONST 1 </strong>

<strong>// Instruction 3: INSERT 1 </strong>

<h1><a name="_Toc12430"></a>// Instruction 4: INSERT 2</h1>

<h1><a name="_Toc12431"></a>// Instruction 5: AT 0</h1>

<strong>// Instruction 6: ADDCONST 1 </strong>

<strong>// Instruction 7: SUBMEM 2 </strong>

<h1><a name="_Toc12432"></a>// Instruction 8: JUMPNZERO 2 // Instruction 9: JUMPZERO 9</h1>

<h1><a name="_Toc12433"></a>// Instruction 10: CLEAR 0</h1>

<strong>// Instruction 11: AT 1 </strong>

<strong>// Instruction 12: MULCONST -2 </strong>

<strong>// Instruction 13: SET 1 </strong>

<strong>// Instruction 14: AT 2 </strong>

<h1><a name="_Toc12434"></a>// Instruction 15: ADDCONST 1</h1>

<h1><a name="_Toc12435"></a>// Instruction 16: SET 2</h1>

<h1><a name="_Toc12436"></a>// Instruction 17: JUMPREL -12</h1>

<strong>// Instruction 18: AT 1 </strong>

<strong>// Instruction 19: MULCONST 3 </strong>

<strong>// Instruction 20: ADDCONST 4 </strong>

<strong>// Instruction 21: SET 1 </strong>

<strong>// Instruction 22: HALT 0 </strong>

<strong>vm.run(); </strong>

<strong>// Status: HALTED </strong>

<a href="#_Toc12429">// Accumulator: -98300                                                </a>

<a href="#_Toc12430">// Instruction 4: INSERT                                             2 </a>

<a href="#_Toc12431">// Instruction 5: AT 0 // Instruction 6: ADDCONST 1 // Instruction 7: SUBMEM                                                                    2 </a>

<a href="#_Toc12432">// Instruction 8: JUMPNZERO 2 // Instruction 9: JUMPZERO 9             </a>

<a href="#_Toc12433">// Instruction 10: CLEAR 0 // Instruction 11: AT 1 // Instruction 12: MULCONST -2 // Instruction 13: SET 1 // Instruction 14: AT           2 </a>

<a href="#_Toc12434">// Instruction 15: ADDCONST 1                                         </a>

<a href="#_Toc12435">// Instruction 16: SET                                               2 </a>

<a href="#_Toc12436">// Instruction 17: JUMPREL -12 // Instruction 18: AT 1 // Instruction 19: MULCONST 3 // Instruction 20: ADDCONST                               4 </a>




<strong>// *** Data Memory *** </strong>

<strong>// Location 0: 15 </strong>

<strong>// Location 1: -98300 </strong>

<strong>// Location 2: 16 </strong>

<strong>// *** Instruction Memory *** </strong>

<strong>// Instruction 0: CHECKMEM 1 </strong>

<strong>// Instruction 1: CLEAR 0 </strong>

<strong>// Instruction 2: ADDCONST 1 </strong>

<strong>// Instruction 3: INSERT 1 </strong>

<strong>// Instruction 21: SET 1 </strong>

<strong>// Instruction 22: HALT 0 std::vector&lt;long&gt; outmem = vm.getDataMem(); </strong>

<strong>vm.reset(); // Status: WAITING </strong>

<strong>// Accumulator: 0 </strong>

<strong>// *** Data Memory *** </strong>

<strong>// *** Instruction Memory *** </strong>

<strong>Deliverables </strong>

Please submit complete projects as zipped folders. The zipped folder should contain:

<ul>

 <li>cpp (Your written GritVM class)</li>

 <li>hpp (Your written GritVM class)</li>

 <li>cpp (The provided base class, enums, structs, and helper functions)</li>

 <li>hpp (The provided base class, enums, structs, and helper functions)</li>

 <li>cpp (Test file)</li>

 <li>hpp (Catch2 Header)</li>

 <li>gvm (A Alternating Sequence Program for GritVM)</li>

 <li>gvm (A Summation Program for GritVM)</li>

 <li>gvm (A Surface Area Program for GritVM)</li>

 <li>gvm (A Test Program for GritVM)</li>

</ul>

And any additional source and header files needed for your project.

<strong>Hints </strong>

Remember, the math can only be done to the accumulator. You will never be doing the four functions on data memory locations. For example, the SUBMEM instruction should perform accumulator = accumulator – dataMem[instruct.argument]

I suggest you write two private methods for your GritVM, evaluate and advance. While the machine is RUNNING, the evaluate method contains a switch statement for every instruction in the instruction set and does the proper functionality. The advance method moves the current instruction by the proper amount. This way, you can have something like: <strong>while(machineStatus == RUNNING) { </strong>

<strong>    // Evaluate the current instruction     long jumpDistance = evaluate(*currentInstruct); </strong>

<strong>    // Advance the current instruction based on the last evaluation </strong>

<strong>    advance(jumpDistance); } </strong>

It would be a good idea to have a public method that can print out all of the GritVM variables (like above in the example). I suggest something like:

<strong>void GritVM::printVM(bool printData, bool printInstruction) { </strong>

<strong>  cout &lt;&lt; “****** Output Dump ******” &lt;&lt; endl;   cout &lt;&lt; “Status: ” &lt;&lt;  </strong>

<strong>          GVMHelper::statusToString(machineStatus) &lt;&lt; endl;   cout &lt;&lt; “Accumulator: ” &lt;&lt; accumulator &lt;&lt; endl; </strong>

<strong>  if (printData) { </strong>

<strong>    cout &lt;&lt; “*** Data Memory ***” &lt;&lt; endl; </strong>

<strong>    int index = 0; </strong>

<strong>    vector&lt;long&gt;::iterator it = dataMem.begin(); </strong>

<strong>    while(it != dataMem.end()) {       long item = (*it); </strong>

<strong>      cout &lt;&lt; “Location ” &lt;&lt; index++ &lt;&lt; “: ” &lt;&lt; item &lt;&lt; endl; </strong>

<strong>      it++; </strong>

<strong>    }   } </strong>

<strong>  if (printInstruction) { </strong>

<strong>    cout &lt;&lt; “*** Instruction Memory ***” &lt;&lt; endl;     int index = 0; </strong>

<strong>    list&lt;Instruction&gt;::iterator it = instructMem.begin(); </strong>

<strong>    while(it != instructMem.end()) { </strong>

<strong>      Instruction item = (*it);       cout &lt;&lt; “Instruction ” &lt;&lt;  </strong>

<strong>               index++ &lt;&lt; “: ” &lt;&lt;  </strong>

<strong>               GVMHelper::instructionToString(item.operation) &lt;&lt;  </strong>

<strong>               ” ” &lt;&lt; item.argument &lt;&lt; endl;       it++; </strong>

<strong>    } </strong>

<strong>  } </strong>

<strong>} </strong>

Try looking through the code of the GVM files and see if you can reverse engineer the functionality.

I would start by getting your load GritVM to load in a GVM file and make sure that all the instruction memory and data memory are working before moving forward. After that, you can work on the run loop that evaluates instructions and moves the current instruction forward.

<strong>Extra Credit </strong>

There are two opportunities for extra credit on this project.

The first is to write a program in the GVM language that calculates the minimum number of moves for a Towers of Hanoi (TOH) solution. The memory layout of the program after halting should be [N, Result] where N was the number of disks passed into the program and Result is the minimum number of steps. Try to reverse engineering other GVM programs to get your TOH program working. A working program is worth 10 extra points and is defined in the second test case.

The second opportunity is to write your own custom implementation of the list and vector used for the GritVM. You will have to be able to convert to and from the Standard Template Versions of the list and vector when taking in or outputting instruction/data memory. This will also be worth an extra 10 points.

Since these are extra credit opportunities, I will not be answering questions on the implementation details. I will only clarify questions on the requirements of the extra credit.


