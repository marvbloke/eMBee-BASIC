eMBee BASIC
===========
Now you can build your own 80's pocket computer! More info on the **eMBee ONE** pocket computer at the project's home on Hackaday.io: https://hackaday.io/project/177180-embee-one-pocket-computer

Based on Robin Edwards' excellent [Arduino BASIC](https://github.com/robinhedwards/ArduinoBASIC), this is the software for a complete pocket computer, using an Arduino Nano or ATMega328P chip, a CardKB I2C keyboard, and SPI OLED screen. The BASIC supports almost all the usual features, with float and string variables, multi-dimensional arrays, FOR-NEXT, GOSUB-RETURN, etc. Saving and loading from internal and external EEPROM is supported, as well as auto-running a program on power-up. You can also read and write from the analog and digital pins.

There's about 1k of RAM available for your BASIC programs and variables, so its roughly equivalent to a Sinclair ZX81. The other 1k of RAM is used for the keyboard and screen buffers, with a small bit of room left for the CPU stack. That works out quite well, since there's a 1k EEPROM on the Nano so if your program fits in the basic environment, it will fit when saved to internal EEPROM.

Prerequisites
-------------
1: An ATMega328P or Arduino with at least 2k of RAM. i.e. Nano/UNO/Duemilanove/etc. 

2: A [CardKB keyboard](https://www.okdo.com/p/cardkb-mini-keyboard-unit-mega328p/?cm_mmc=UK-PLA-DS3A-_-google&&campaignid=11705773582&adgroupid=116689368627&network=g&device=m&product_partition_id=983775196933&product_id=2027611-gb&gclid=CjwKCAiAt9z-BRBCEiwA_bWv-H259FRGtjNEjpWTQtWl0V5uXgjzWPzhluYvt18KG0IBjgX63514HBoCl2YQAvD_BwE&gclsrc=aw.ds) 

3: An SSD1306 based OLED screen connected using SPI. Runs at 128x64 resolution in text mode (21 characters wide by 8 characters tall).

4: An Altoids tin to make it a truly portable device. I use a PP3 9-volt battery connected to the ATMega328P via an LM7805 regulator.

5: (Optional) A Piezoelectric buzzer for keyboard clicks and other sounds.

6: (Optional) A external EEPROM (e.g. 24LC256 giving 32KB storage) lets you save more than one file. 

Getting Started
---------------
1: Download the zip file, unpack and copy the *folder* to your arduino sketches directory.

2: Go to Manage Libraries, search for 'Digital Infrared Temperature Sensor MLX90615' and install it (in order to get 'I2CMaster')

3: Install the SSD1306ASCII library (updated by Robin Edwards for speed - included in package)

4: Check your wiring corresponds to the pins in the comments/defines at the top of the eMBee_BASIC file.

BASIC Language
--------------
Variables names can be up to 8 alphanumeric characters but start with a letter e.g. a, bob32
String variable names must end in $ e.g. a$, bob32$
Case is ignored (for all identifiers). BOB32 is the same as Bob32. print is the same as PRINT

Array variables are independent from normal variables. So you can use both:
```
LET a = 5
DIM a(10)
```
There is no ambiguity, since a on its own refers to the simple variable 'a', and a(n) referes to an element of the 'a' array.

```
Arithmetic operators: + - * / MOD
Comparisons: <> (not equals) > >= < <=
Logical: AND, OR, NOT
```

Expressions can be numerical e.g. 5*(3+2), or string "Hello "+"world".
Only the addition operator is supported on strings (plus the functions below).

Commands
```
PRINT <expr>;<expr>... e.g. PRINT "A=";a
LET variable = <expr> e.g. LET A$="Hello"
variable = <expr> e.g. A=5
LIST [start],[end] e.g. LIST or LIST 10,100
RUN [lineNumber]
GOTO lineNumber
REM <comment> e.g. REM ** My Program ***
STOP
CONT (continue from a STOP)
INPUT variable e.g. INPUT a$ or INPUT a(5,3)
IF <expr> THEN cmd e.g. IF a>10 THEN a = 0: GOTO 20
FOR variable = start TO end STEP step e.g. FOR n=1 TO 10: PRINT n: NEXT n
NEXT variable
NEW
GOSUB lineNumber
RETURN
DIM variable(n1,n2...)
CLS
BEEP [pitch],[duration] (pitch is 1=high, 2=mid, 3=low, duration varies depending on pitch e.g. BEEP 1,100 is a short high beep) 
PAUSE milliseconds
RAND (re-seeds the random number generator using time elapsed)
AT y,x sets the cursor (this is a separate command rather than PRINT AT). Starts at 0,0 in top-left and goes to 7,20
PIN pinNum, value (0 = low, non-zero = high)
PINMODE pinNum, mode ( 0 = input, 1 = output)
LOAD (from internal EEPROM)
SAVE (to internal EEPROM) e.g. use SAVE + to set to auto-run
LOAD "filename", SAVE "filename, DIR, DELETE "filename", FORMAT if using with external EEPROM.
SEND "string" - sends a string via UART interface
```

"Pseudo-identifiers"
```
INKEY$ - returns (and eats) the last key pressed buffer (non-blocking). e.g. PRINT INKEY$
RECV$ - receives a character from UART interface
RND - random number betweeen 0 and 1. e.g. LET a = RND
PI - 3.141593
```

Functions
```
LEN(string) e.g. PRINT LEN("Hello") -> 5
VAL(string) e.g. PRINT VAL("1+2") -> 3
INT(number) e.g. INT(1.5) -> 1
ABS(number) e.g. ABS(-3.5) -> 3.5
SIN/COS/TAN(radians) - trigonometric functions
ARCSIN/ARCCOS/ARCTAN(number) - inverse trigonometric functions
EXP(number) - returns the exponential number
LN(number) - returns the natural logarithm of a number
SQR(number) e.g. SQR(9) -> 3
STR$(number) e.g. STR$(2) -> "2"
LEFT$(string,n)
RIGHT$(string,n)
MID$(string,start,n)
CHR$(number) e.g. CHR$(65) -> "A"
CODE(char) e.g. CODE("A") -> 65
PINREAD(pin) - see Arduino digitalRead()
ANALOGRD(pin) - see Arduino analogRead()
```

Still to come
```
Sending/receiving programs via UART
