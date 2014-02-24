
POWER SWITCH WITH I2C CONTROL
=============================

Connection to the Raspberry Pi I2C bus is done as follows

Raspberry Pi     power switch CONN4     
SDA (GPIO0) ---- 2 SDA
SCL (GPIO1) ---- 4 SCL

Ground connection is not needed in a typical application. If the ground is 
connected from GPIO on the Raspberry Pi be careful to connect it to the correct
pin at the header. Large currents can flow in the ground wire and these could 
damage the System-on-Chip (SoC) on the Raspberry Pi if wrong connections are 
done. Note also that the two ground potentials to be connected are not 
necessarily exactly at the same level. 

When the Raspberry Pi is powered down and the PIC is still supplied by the
3.0 V regulator a small current of about 30 uA will continue to flow through
R5 and R6 to the Raspberry Pi SDA and SCL pins.

The board_mirror.pdf can be printed or copied with a laser printer and 
transferred with iron on a copper clad glassfiber board for etching. Sharp
knife and multimeter are needed to fix any possible shorts on the produced
PCB. Three ground jumper wires should also be soldered since the PCB is one
sided for easier production.

The circuit is being tested at the time of writing this README file. Building 
needs good understanding of the circuit and good skills in electronics. 


Parts
-----

C1,C2,C5   100nF
C3         2.2uF/25V
C4         1uF/35V
CONN1      Anderson powerpole connector
CONN2      Anderson powerpole connector
CONN3      Anderson powerpole connector
CONN4      modular connector 4P4C
D1         LED 3 mm	
D2         LED 3 mm	
F1         UF185 polyfuse (1.85 A) or microfuse 
F2         UF185 polyfuse (1.85 A) or microfuse 
J1	   5 pins header 
J2,J3,J4,J5,J6   2 pins header
Q1,Q3,Q5   2N3904
Q2,Q4      IRF9530
Q6,Q7	   BS170
R1,R4      10k
R2,R5      2k2
R3,R6,R9,R10    100k
R7         1k
R8         ~ 1 M
S1,S2      PCB button
U1         PIC12F675
U2         LP2950-3.0

Files
-----

board_mirror.pdf         - PCB for toner transfer production
board_silk_mirror.pdf    - silk screen for toner transfer printing
board_component.pdf      - PCB component placement
raspipwrswitch           - project definition file
raspipwrswitch.pdf       - circuit diagram
raspipwrswitch.png
raspipwrswitch.sch
README.txt               - this file


Software
--------

The PIC is programmed with the code in repository ('powerswitch' branch)

PiPIC/12f675/pic12si2c/pic12si2c.asm

The LED blinking code indicating an active timed tasks needs to be commented 
out from the file above.

The EEPROM needs to have following data

address   data      bits inverted except for TRISIO
10        F8        ini_CMCON
11        F4        ini_GPIO
12        FB        ini_ADCON0
13        AF        ini_ANSEL
14        FF        ini_VRCON
15        0F        ini_TRISIO
16        CE        ini_T1CON
17        FC        ini_IOC
20        27        i2c address
21        80        enable event triggered tasks
22        34        GP0 low task command: toggle GP4 
23        10
24        34        GP1 low task command: toggle GP5
25        20

See the manual page pipic(1) on how this data could be programmed.

At the Raspberry Pi the sever pipicswd(1) and client pipicsw(1) can be used 
to operate the switch and read its status.

