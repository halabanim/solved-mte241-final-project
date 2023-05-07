Download Link: https://assignmentchef.com/product/solved-mte241-final-project
<br>
The final assessment is a uVision5 project that solves the Byzantine Generals Problem.  You will implement the part of the OM algorithm that distributes the values to the generals.  You do not need to calculate majority values (i.e. each general does not have to reach a decision).  Your program will output the values received in the OM(0) stage by only one of the generals.  To do the assessment, download the starter project, final.zip.  It includes three source files:

<ul>

 <li>c – the test code (details below),</li>

 <li>h – macro definitions and function declarations, • general.c – the file in which you implement your solution.</li>

</ul>

Submit only general.c to the Learn dropbox.  The given test cases will be used to do the grading with only these variations: changing which general is the commander, changing what command is sent, or changing which general generates output.

<h1>Test Thread</h1>

<table width="665">

 <tbody>

  <tr>

   <td width="665">void testCases(void *arguments) {   for(int i=0; i&lt;N_TEST; i++) {     printf(“
test case %d
”, i);if(setup(tests[i].n, tests[i].loyal, tests[i].reporter)) {startGenerals(tests[i].n);broadcast(tests[i].command, tests[i].commander);cleanup();       stopGenerals();} else {printf(” setup failed
”);}}printf(“
done
”);}</td>

  </tr>

 </tbody>

</table>

The test thread iterates through 6 test cases, increasing in complexity.  For each test case, it calls your setup() function first to record the test parameters and create any OS resources or other resources that will be needed by your solution.  Setup() should calculate m, the number of traitors, and use c_assert() to test that n &gt; 3*m and return false if fails  The test thread then starts n general threads with IDs 0 … n-1.  The general threads run your general() function.  Then it invokes your broadcast() function which sends a command from the commanding general to the other generals.  Your broadcast() function should return when the algorithm completes and the expected output has been printed to UART #1.  When your broadcast() function returns, the test thread invokes your cleanup() function to delete or free any resources and then the test threads deletes the n general threads.

<h1>Test Cases</h1>

<table width="665">

 <tbody>

  <tr>

   <td width="70">Case</td>

   <td width="98"># generals</td>

   <td width="140">Loyal?<sup>*</sup></td>

   <td width="119">Reporter</td>

   <td width="119">Command</td>

   <td width="119">Commander</td>

  </tr>

  <tr>

   <td width="70">0</td>

   <td width="98">3</td>

   <td width="140">T, T, F</td>

   <td width="119">1</td>

   <td width="119">R</td>

   <td width="119">0</td>

  </tr>

  <tr>

   <td width="70">1</td>

   <td width="98">3</td>

   <td width="140">T, T, T</td>

   <td width="119">2</td>

   <td width="119">R</td>

   <td width="119">0</td>

  </tr>

  <tr>

   <td width="70">2</td>

   <td width="98">3</td>

   <td width="140">T, T, T</td>

   <td width="119">2</td>

   <td width="119">A</td>

   <td width="119">1</td>

  </tr>

  <tr>

   <td width="70">3</td>

   <td width="98">4</td>

   <td width="140">T, F, T, T</td>

   <td width="119">2</td>

   <td width="119">R</td>

   <td width="119">0</td>

  </tr>

  <tr>

   <td width="70">4</td>

   <td width="98">4</td>

   <td width="140">T, F, T, T</td>

   <td width="119">2</td>

   <td width="119">A</td>

   <td width="119">1</td>

  </tr>

  <tr>

   <td width="70">5</td>

   <td width="98">7</td>

   <td width="140">T, F, T, T, F, T, T</td>

   <td width="119">6</td>

   <td width="119">R</td>

   <td width="119">0</td>

  </tr>

 </tbody>

</table>

<sup>*</sup>T = true, F = false

Test case 0 is n=3, m=1 so asserting n &gt; 3*m should fail.

Test case 1 is n=3, m=0.  General 0 sends command R.  General 2 reports what it receives in OM(0).

Test case 2 is n=3, m=0.  General 1 sends command A.  General 2 reports.

Test case 3 is n=4, m=1.  General 0 sends command R.  General 2 reports.  General 1 is a traitor.

Test case 4 is n=4, m=1.  General 1 sends command A.  General 2 reports.  General 1 is a traitor. Test case 5 is n=7, m=2.  General 0 sends command R.  General 6 reports.  Generals 1 and 4 are traitors.

<h1>Traitors</h1>

A treacherous commander will broadcast ‘R’ to even numbered generals and ‘A’ to odd numbered generals.

In all subsequent steps, a treacherous lieutenant will send ‘R’ to all other generals if it is even numbered and ‘A’ otherwise.

Loyal generals will send the same command that they receive.

<h1>Expected Output</h1>

<table width="665">

 <tbody>

  <tr>

   <td width="665">test case 0general.c,22: assertion ‘n &gt; 3*m’ failed  setup failedtest case 1  0:Rtest case 2  1:Atest case 3  1:0:A 3:0:Rtest case 4  0:1:R 3:1:Atest case 52:1:0:A 3:1:0:A 4:1:0:R 5:1:0:A 1:2:0:A 2:3:0:R 3:2:0:R 4:2:0:R5:2:0:R 5:3:0:R 1:3:0:A 4:3:0:R 3:4:0:R 5:4:0:R 1:4:0:A 2:4:0:R4:5:0:R 1:5:0:A 2:5:0:R 3:5:0:R done</td>

  </tr>

 </tbody>

</table>

The order that output values are listed for each test case is not important.  The values themselves are important.  For example, if the test case 5 output above was sorted and put in table form, it would correspond to:

-:-:-:- 1:2:0:A 1:3:0:A 1:4:0:A 1:5:0:A -:-:-:-

2:1:0:A -:-:-:- 2:3:0:R 2:4:0:R 2:5:0:R -:-:-:-

3:1:0:A 3:2:0:R -:-:-:- 3:4:0:R 3:5:0:R -:-:-:-

4:1:0:R 4:2:0:R 4:3:0:R -:-:-:- 4:5:0:R -:-:-:-

5:1:0:A 5:2:0:R 5:3:0:R 5:4:0:R -:-:-:- -:-:-:-

-:-:-:- -:-:-:- -:-:-:- -:-:-:- -:-:-:-