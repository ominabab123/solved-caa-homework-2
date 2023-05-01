Download Link: https://assignmentchef.com/product/solved-caa-homework-2
<br>
<h1>Handwritten</h1>

<h2>2.8</h2>

Translate the following RISC-V code to C. Assume that the variables f, g, h, i, and j are assigned to registers x5, x6, x7, x28, and x29, respectively. Assume that the base address of the arrays A and B are in registers x10 and x11, respectively.

addi x30, x10, 8 addi x31, x10, 0 sd x31, 0(x30) ld x30, 0(x30) add x5, x30, x31

<h2>2.9</h2>

For each RISC-V instruction in Exercise 2.8, show the value of the opcode (op), source register (rs1), and destination register (rd) fields. For the I-type instructions, show the value of the immediate field, and for the R-type instructions, show the value of the second source register (rs2). For non U- and UJ-type instructions, show the funct3 field, and for R-type and S-type instructions, also show the funct7 field.

<h2>2.16</h2>

Assume that we would like to expand the RISC-V register file to 128 registers and expand the instruction set to contain four times as many instructions.

<strong>2.16.1 </strong>

How would this affect the size of each of the bit fields in the R-type instructions?

<strong>2.16.2 </strong>How would this affect the size of each of the bit fields in the I-type instructions?

<h3>2.16.3 (</h3>

How could each of the two proposed changes decrease the size of a RISC-V assembly program? On the other hand, how could the proposed change increase the size of an RISC-V assembly program?

<h1>2        Programming</h1>

We will test the following problems on <a href="http://palms.ee.princeton.edu/system/files/HPCA2015_3_software.pdf">RISC-V software stack</a><a href="http://palms.ee.princeton.edu/system/files/HPCA2015_3_software.pdf">.</a> The packages we use are <a href="https://github.com/riscv/riscv-isa-sim">spike</a><a href="https://github.com/riscv/riscv-isa-sim">,</a> <a href="https://github.com/riscv/riscv-pk">proxy kernel </a>with newlib and <a href="https://github.com/riscv/riscv-gnu-toolchain">gcc</a><a href="https://github.com/riscv/riscv-gnu-toolchain">.</a> And the riscv-isa we will use to test the program is <a href="https://en.wikipedia.org/wiki/RISC-V">RV64IMAFDC</a><a href="https://en.wikipedia.org/wiki/RISC-V">.</a>

Before we start programming, we will use <a href="https://www.docker.com/">docker</a> to set up our environment (Refer to the supplementary.pdf to see how to install docker and use it).

docker pull ntuca2020/hw2 // (4G) docker run –name=test -it ntuca2020/hw2 cd /root ls

The folder structure in the docker image looks like the following:

<table width="391">

 <tbody>

  <tr>

   <td width="209">/root|– Examples</td>

   <td width="181"></td>

  </tr>

  <tr>

   <td width="209">         |          |– Example1</td>

   <td width="181">// inline assembly test</td>

  </tr>

  <tr>

   <td width="209">         |          |– Example2</td>

   <td width="181">// link with .s file test</td>

  </tr>

  <tr>

   <td width="209">         |          ‘– Example3‘– Problems</td>

   <td width="181">// setup debug environment</td>

  </tr>

  <tr>

   <td width="209">|– fibonacci|           |– Makefile|            |– fibonacci.c|             ‘– fibonacci.s</td>

   <td width="181">// fibonacci number</td>

  </tr>

  <tr>

   <td width="209">|– convert|           |– Makefile|           |– convert.c|            ‘– convert.s</td>

   <td width="181">// string to int</td>

  </tr>

  <tr>

   <td width="209">‘– matrix|– Makefile|– matrix.c‘– matrix.s</td>

   <td width="181">// matrix multiplcation</td>

  </tr>

 </tbody>

</table>

make and make test to try it out.

You only need to submit *.s file of each problem.

<h2>Fibonacci</h2>

Implement Fibonacci number in assembly. (<em>F</em><sub>0 </sub>=0<em>,F</em><sub>1 </sub>=1, output <em>F<sub>n</sub></em>, no overflow)

unsigned long long int fibonacci(int);

Input:                                                                                                Output:

70                                                                                                       190392490709135

<h2>Convert</h2>

<ul>

 <li>Convert an ASCII string containing a positive or negative integer decimal string to an integer. Input length is at most 15 bytes.</li>

 <li>‘+’ and ‘-’ will appear optionally. And once they appear, they will only appear once in the first byte.</li>

 <li>If a non-digit character appears anywhere in the string, your program should stop and return -1.</li>

 <li>The return value will be printed out in 32bit-int format.</li>

</ul>

int convert(char *);

Input:                                                                                                Output:

+123                                                                                                  123

+0000000123                                                                                  123

-123                                                                                                   -123

-0000000321                                                                                   -321

2147483647                                                                                    2147483647

2147483648                                                                                     -2147483648

-2147483648                                                                                   -2147483648

-123123AAA                                                                                     -1

+123123AAA                                                                                    -1

123123AAA                                                                                      -1

<h2>Matrix multiplication (15%)</h2>

Do matrix multiplication of size 128×128 with some additional operations.

for (int i = 0; i &lt; SIZE; i++) for (int j = 0; j &lt; SIZE; j++) for (int k = 0; k &lt; SIZE; k++)

C[i][j] = (C[i][j] + A[i][k] * B[k][j]) % 1024; Elements in A and B are unsigned 16 bits numbers of range [0,1023] We will score based on the cycle counts. You can use C as an initial attempt.

asm volatile (“rdcycle %0” : “=r” (start));

// matrix multiplication asm volatile (“rdcycle %0” : “=r” (end));

Grading:

<ul>

 <li>Below 20,000,000 cycles (2%)</li>

 <li>Below 18,000,000 cycles (2%)</li>

 <li>Below 16,000,000 cycles (2%)</li>

 <li>Below 14,000,000 cycles (2%)</li>

 <li>Below 12,000,000 cycles (2%)</li>

 <li>Below 10,000,000 cycles (1%)</li>

 <li>Below 9,000,000 cycles (1%)</li>

 <li>Below 8,000,000 cycles (1%)</li>

 <li>Below 7,000,000 cycles (1%)</li>

 <li>Below 6,000,000 cycles (1%)</li>

</ul>

<h2>Report on matrix multiplication (15%)</h2>

<ul>

 <li>Briefly explain how you get below 6,000,000 cycles.</li>

 <li>Or you can answer the following questions:

  <ul>

   <li>How many cycles does it take by just doing the naive matrix multiplication?</li>

   <li>How many load and store does it need (roughly) during the whole computation? (Considering the registers it use)</li>

   <li>Is there any way to keep registers being used as much as possible before they’re replaced? (Hint: blocking)</li>

   <li>How many loop controls does it need (roughly) during the whole computation?<h1>           Handwritten (35%)</h1><h2>2.8 (10%)</h2>Translate the following RISC-V code to C. Assume that the variables f, g, h, i, and j are assigned to registers x5, x6, x7, x28, and x29, respectively. Assume that the base address of the arrays A and B are in registers x10 and x11, respectively.addi x30, x10, 8 addi x31, x10, 0 sd x31, 0(x30) ld x30, 0(x30) add x5, x30, x31<h2>2.9 (10%)</h2>For each RISC-V instruction in Exercise 2.8, show the value of the opcode (op), source register (rs1), and destination register (rd) fields. For the I-type instructions, show the value of the immediate field, and for the R-type instructions, show the value of the second source register (rs2). For non U- and UJ-type instructions, show the funct3 field, and for R-type and S-type instructions, also show the funct7 field.<h2>2.16 (15%)</h2>Assume that we would like to expand the RISC-V register file to 128 registers and expand the instruction set to contain four times as many instructions.<strong>2.16.1 (5%)</strong>How would this affect the size of each of the bit fields in the R-type instructions?<strong>2.16.2 (5%)</strong>How would this affect the size of each of the bit fields in the I-type instructions?<h3>2.16.3 (5%)</h3>How could each of the two proposed changes decrease the size of a RISC-V assembly program? On the other hand, how could the proposed change increase the size of an RISC-V assembly program?<h1>2        Programming</h1>We will test the following problems on <a href="http://palms.ee.princeton.edu/system/files/HPCA2015_3_software.pdf">RISC-V software stack</a><a href="http://palms.ee.princeton.edu/system/files/HPCA2015_3_software.pdf">.</a> The packages we use are <a href="https://github.com/riscv/riscv-isa-sim">spike</a><a href="https://github.com/riscv/riscv-isa-sim">,</a> <a href="https://github.com/riscv/riscv-pk">proxy kernel </a>with newlib and <a href="https://github.com/riscv/riscv-gnu-toolchain">gcc</a><a href="https://github.com/riscv/riscv-gnu-toolchain">.</a> And the riscv-isa we will use to test the program is <a href="https://en.wikipedia.org/wiki/RISC-V">RV64IMAFDC</a><a href="https://en.wikipedia.org/wiki/RISC-V">.</a>Before we start programming, we will use <a href="https://www.docker.com/">docker</a> to set up our environment (Refer to the supplementary.pdf to see how to install docker and use it).docker pull ntuca2020/hw2 // (4G) docker run –name=test -it ntuca2020/hw2 cd /root lsThe folder structure in the docker image looks like the following:

    <table width="391">

     <tbody>

      <tr>

       <td width="209">/root|– Examples</td>

       <td width="181"></td>

      </tr>

      <tr>

       <td width="209">         |          |– Example1</td>

       <td width="181">// inline assembly test</td>

      </tr>

      <tr>

       <td width="209">         |          |– Example2</td>

       <td width="181">// link with .s file test</td>

      </tr>

      <tr>

       <td width="209">         |          ‘– Example3‘– Problems</td>

       <td width="181">// setup debug environment</td>

      </tr>

      <tr>

       <td width="209">|– fibonacci|           |– Makefile|            |– fibonacci.c|             ‘– fibonacci.s</td>

       <td width="181">// fibonacci number</td>

      </tr>

      <tr>

       <td width="209">|– convert|           |– Makefile|           |– convert.c|            ‘– convert.s</td>

       <td width="181">// string to int</td>

      </tr>

      <tr>

       <td width="209">‘– matrix|– Makefile|– matrix.c‘– matrix.s</td>

       <td width="181">// matrix multiplcation</td>

      </tr>

     </tbody>

    </table>make and make test to try it out.You only need to submit *.s file of each problem.<h2>Fibonacci</h2>Implement Fibonacci number in assembly. (<em>F</em><sub>0 </sub>=0<em>,F</em><sub>1 </sub>=1, output <em>F<sub>n</sub></em>, no overflow)unsigned long long int fibonacci(int);Input:                                                                                                Output:70                                                                                                       190392490709135<h2>Convert</h2>

    <ul>

     <li>Convert an ASCII string containing a positive or negative integer decimal string to an integer. Input length is at most 15 bytes.</li>

     <li>‘+’ and ‘-’ will appear optionally. And once they appear, they will only appear once in the first byte.</li>

     <li>If a non-digit character appears anywhere in the string, your program should stop and return -1.</li>

     <li>The return value will be printed out in 32bit-int format.</li>

    </ul>int convert(char *);Input:                                                                                                Output:+123                                                                                                  123+0000000123                                                                                  123-123                                                                                                   -123-0000000321                                                                                   -3212147483647                                                                                    21474836472147483648                                                                                     -2147483648-2147483648                                                                                   -2147483648-123123AAA                                                                                     -1+123123AAA                                                                                    -1123123AAA                                                                                      -1<h2>Matrix multiplication</h2>Do matrix multiplication of size 128×128 with some additional operations.for (int i = 0; i &lt; SIZE; i++) for (int j = 0; j &lt; SIZE; j++) for (int k = 0; k &lt; SIZE; k++)C[i][j] = (C[i][j] + A[i][k] * B[k][j]) % 1024; Elements in A and B are unsigned 16 bits numbers of range [0,1023] We will score based on the cycle counts. You can use C as an initial attempt.asm volatile (“rdcycle %0” : “=r” (start));// matrix multiplication asm volatile (“rdcycle %0” : “=r” (end));Grading:

    <ul>

     <li>Below 20,000,000 cycles (2%)</li>

     <li>Below 18,000,000 cycles (2%)</li>

     <li>Below 16,000,000 cycles (2%)</li>

     <li>Below 14,000,000 cycles (2%)</li>

     <li>Below 12,000,000 cycles (2%)</li>

     <li>Below 10,000,000 cycles (1%)</li>

     <li>Below 9,000,000 cycles (1%)</li>

     <li>Below 8,000,000 cycles (1%)</li>

     <li>Below 7,000,000 cycles (1%)</li>

     <li>Below 6,000,000 cycles (1%)</li>

    </ul><h2>Report on matrix multiplication</h2>

    <ul>

     <li>Briefly explain how you get below 6,000,000 cycles.</li>

     <li>Or you can answer the following questions:

      <ul>

       <li>How many cycles does it take by just doing the naive matrix multiplication?</li>

       <li>How many load and store does it need (roughly) during the whole computation? (Considering the registers it use)</li>

       <li>Is there any way to keep registers being used as much as possible before they’re replaced? (Hint: blocking)</li>

       <li>How many loop controls does it need (roughly) during the whole computation?</li>

      </ul></li>

    </ul></li>

  </ul></li>

</ul>