# Password-based-doorlock-system-in-8051-microprocessor

### software requirements

- Proteus (for circuit diagram and simulation)
- Keil uVision5 IDE (for c code)

### Hardware requirements:
- 8051 microcontroller (80C51)
- Phone keypad
- LCD – LM016L
- DC Motor (re-present as a door-lock motor)
  
### Objective: 

<p> Safety is the most crucial concern of human. We always try to keep our things between ourselves. For this reason, we still use various methods to lock our precious items like a locked diary. And when it comes to our daily life, we are more serious. In the modern age, there are so many ways to lock the door; one of them is password-based lock system—a system where you are the only one to know how to access it. It saves our daily life from the various malicious problem like a thief. This system will give us the security that we want. To make our life more secure, we are going to build the password-based door lock system. This system is easy to assemble and very easy to use in our daily life. Anyone can use it to secure themselves.</p>


### Applications: 

<p>The password-based lock system can install any door of any rooms. This system also can be integrated with the existing system. This electric combination lock system uses a five-digit password. The system collects five-digit user input and compares the user input with the preset password inside the program. If the password matches, access will be granted, and if not match the entry will be denied. The system can be used at residential places to ensure better safety. It can be used at organizations to ensure authorized access to highly secured places.</p>

### Block Diagram:

![Block Diagram](https://i.imgur.com/uV6oYkP.png)

<p>Fig: System Block Diagram</p>


### Working Procedures:
1.	The main component in the circuit is 8051 microcontrollers. This control everything in the device.
2.	We connect the 4x3 keypad with the microcontroller. In the keypad there is 4 rows which is indicated the letters (A-D) and the 3 columns which is indicated the number (1-3).
3.	In the 8051 controller, pin P2.0 to P2.3 are connected to the keypad rows and pin P3.0 to P3.2 are connected to the keypad columns.
4.	Then we connect the door lock motor pins in the P3.3 and P3.4. This motor only work when the password is right.
5.	LCD’s points are connected with P3.5 to P3.7 in 8051.  LCD help us to show the inputs and error messages.
6.	All devices are connected with each other. In the simulation we use the hex code to run the program. The hex code is generated from the embedded C code. This code only works on any 8051-microcontroller system. 
7.	To run the simulation, we set the clock frequency at 11.0592 MHZ for 8051. This is allowed us to run the simulation without getting any errors.
8.	We pre-define the password 12345 for the program.
9.	If user input wrong password it will give another chance to input the password again with an error message. When the password is right it will show the welcome message and lock motor will move to open the door.



### Schematic Circuit Diagram:

![Schematic Circuit Diagram](https://i.imgur.com/stAXULK.png)

<p>Fig: Schematic Circuit Diagram</p>


### Project Code:

```c

// password based door lock system in 8051 microprocessor

#include <reg51.h>

// connected pins
// keypad rows
sbit keyrow1 = P2 ^ 0;
sbit keyrow2 = P2 ^ 1;
sbit keyrow3 = P2 ^ 2;
sbit keyrow4 = P2 ^ 3;
//keypad column
sbit keycolumn1 = P3 ^ 0;
sbit keycolumn2 = P3 ^ 1;
sbit keycolumn3 = P3 ^ 2;

// motor pins
sbit motorpin1 = P3 ^ 3;
sbit motorpin2 = P3 ^ 4;

// led pins
sbit rs = P3 ^ 5;
sbit rw = P3 ^ 6;
sbit en = P3 ^ 7;

//functions
void lcdcmd(unsigned char);
void lcddat(unsigned char);
void lcddisplay(unsigned char *q);
char keypad();
void check();
void delay(unsigned int);
unsigned char pin[] = {"12345"};
unsigned char Epin[5];

// main function
void main()
{
    lcdcmd(0x0F); //decimal value: 15
    lcdcmd(0x38); //decimal value: 56
    lcdcmd(0x01); //decimal value: 1

    while (1)
    {
        unsigned int i = 0;
        lcdcmd(0x80); //decimal value: 128
        lcddisplay("ENTER PIN NUMBER");
        delay(1000);
        lcdcmd(0xc0); //decimal value: 192
        while (pin[i] != '\0')
        {
            Epin[i] = keypad();
            delay(1000);
            i++;
        }
        check();
    }
}

//delay function
void delay(unsigned int j)
{
    int a, b;
    for (a = 0; a < j; a++)
    {
        for (b = 0; b < 10; b++)
            ;
    }
}

// lcd commands functions
void lcdcmd(unsigned char A)
{
    P1 = A;
    rs = 0;
    rw = 0;
    en = 1;
    delay(1000);
    en = 0;
}

//lcd data function

void lcddat(unsigned char i)
{
    P1 = i;
    rs = 1;
    rw = 0;
    en = 1;
    delay(1000);
    en = 0;
}

//lcd display charecters function

void lcddisplay(unsigned char *q)
{
    int k;
    for (k = 0; q[k] != '\0'; k++)
    {
        lcddat(q[k]);
    }
    delay(10000);
}

// assign keypad character value function

char keypad()
{
    int x = 0;
    while (x == 0)
    {
        // assign values for first row
        keyrow1 = 0;
        keyrow2 = 1;
        keyrow3 = 1;
        keyrow4 = 1;
        if (keycolumn1 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '1';
        }
        if (keycolumn2 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '2';
        }
        if (keycolumn3 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '3';
        }
        // assign values for second row
        keyrow1 = 1;
        keyrow2 = 0;
        keyrow3 = 1;
        keyrow4 = 1;

        if (keycolumn1 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '4';
        }
        if (keycolumn2 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '5';
        }
        if (keycolumn3 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '6';
        }

        // assign values for third row
        keyrow1 = 1;
        keyrow2 = 1;
        keyrow3 = 0;
        keyrow4 = 1;
        if (keycolumn1 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '7';
        }
        if (keycolumn2 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '8';
        }
        if (keycolumn3 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '9';
        }

        // assign values for forth row
        keyrow1 = 1;
        keyrow2 = 1;
        keyrow3 = 1;
        keyrow4 = 0;

        if (keycolumn1 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '*';
        }
        if (keycolumn2 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '0';
        }
        if (keycolumn3 == 0)
        {
            lcddat('*');
            delay(1000);
            x = 1;
            return '#';
        }
    }
}

// password check function and run the door motor

void check()
{
    //  compare the input value with the assign password value
    if (pin[0] == Epin[0] && pin[1] == Epin[1] && pin[2] == Epin[2] && pin[3] == Epin[3] && pin[4] == Epin[4])
    {
        delay(1000);
        lcdcmd(0x01); //decimal value: 1
        lcdcmd(0x81); //decimal value: 129
        // show pin is correct
        lcddisplay("PIN CORRECT");
        delay(1000);
        // door motor will run
        motorpin1 = 1;
        motorpin2 = 0;
        lcdcmd(0xc1); //decimal value: 193
        // show the door is unlocked
        lcddisplay("DOOR OPENED");
        delay(10000);
        motorpin1 = 1;
        motorpin2 = 0;
        lcdcmd(0x01); //decimal value: 1
    }
    else
    {
        lcdcmd(0x01); //decimal value: 1
        lcdcmd(0x80); //decimal value: 128
        lcddisplay("WRONG PIN");
        delay(1000);
        lcdcmd(0x01); //decimal value: 1
    }
}

// end
```

### Discussion:

<p>In the program first we import the 8051-family header file which contain all the necessary classes. Then we assign every connected pin to the variables. 
In keypad we assign variable as keyrow1 to keyrows4 for keypad rows and keycolumn1 to keycolumn3 for keypad columns. Then we connected the motor pins and lcd pins in the variable called motor pins and rs, rw and en.</p>

<p>After assign all the connected pins to the variables. We declare functions for every single task. The main function calls the other functions for execute the program.</p>

<p>Delay function is used for delay the program execution for few seconds. It helps us to execute the program smoothly. We delay the execution for only 1000ms = 1secs. </p>

<P>The lcdcmd function is help us to control the lcd. It will control the current flow for the lcd screen and also help to perform Read/Write Operation. 
The lcddat function control the lcd data pins. In the LCD there is total 8 pins D0 to D7 Pins used to send Command or data to the LCD.</P>

<p>The lcddisplay function is used for display user input characters and also assign the values for every keypad character values. First, we select a single keypad row and assign the values for the key. Also encoded with the asterisk (*) character, for pin protection.</p>

<p>The check function is used for compare the user input with the assign password number. If the user input match the assign password, it will show the “pin correct” message and give the signal to run the lock motor. After lock motor will run, again it will show the “door open” message. On the other hand, if the user input password is wrong, it will show the “wrong pin” message. Then it will delay few second and give another chance to input the password again.</p>

<p>The main function is the mother function of the program. Main function will call other function in other to perform the task. At first it will call the lcdcmd function to control the lcd, then it will call the lcddisplay function for display the messages in the lcd screen. It will delay for few seconds. After that it will run loop until the correct pin enter. To check the password is correct it will call check function, then execute the program.</p>

<p>To simulate the program circuit, we use the proteus simulation. All the code is written in embedded C code, which is not directly executed by the 8051 microcontrollers. To run the program first we need to build the program and convert the program in hex file. The hex file is supported by the 8051 microcontrollers.  Every 8051 controller has a clock frequency for perform the task. To run the program, we use 11.0592 MHZ clock frequency. It will run the simulation smoothly without any kind of errors.</p>
