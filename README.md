eMBee BASIC
===========
Now you can turn an Altoids tin and an Arduino into an 80's pocket computer!

Based on Robin Edwards' excellent Arduino BASIC, this is a complete pocket computer, using an Arduino Nano (or clone), a CardKB I2C keyboard, and SPI OLED screen. The BASIC supports almost all the usual features, with float and string variables, multi-dimensional arrays, FOR-NEXT, GOSUB-RETURN, etc. Saving and Loading from internal and external EEPROM is supported, as well as auto-running a program on power-up. You can also read and write from the analog and digital pins.

There's about 1k of RAM available for your BASIC programs and variables, so its roughly equivalent to a Sinclair ZX81. The other 1k of RAM (on a Nano) is used for the keyboard and screen buffers, with a small bit of room left for the CPU stack. That works out quite well, since there's a 1k EEPROM on the Nano so if your program fits in the basic environment, it will fit when saved to EEPROM!

Prerequisites
-------------
1: An Arduino with at least 2k of RAM. i.e. Nano/UNO/Duemilanove/etc. It should also work on the MEGA but is untested. I think the sketch is too big for the Leonardo, since it looks like the bootloader uses more RAM on this model.

2: A CardKB keyboard

3: An SSD1306 based OLED Screen connected using SPI. See e.g. http://www.adafruit.com/product/938. I think it should also work fine with the 128x32 version - you'll just need to change a couple of defines.

4: An Altoids tin and some mini breadboards to make it a truly portable device. I used an Adafruit PowerBoost 500 Basic to connect the Nano to 2 x AAA batteries for power.

5: (Optional) A Piezoelectric buzzer for keyboard clicks and other sounds.

6: (Optional) A external EEPROM (e.g. 24LC256 giving 32KB storage) lets you save more than one file. You can pick these up for about £2/$2 on ebay.

Getting Started
---------------
1: Download the zip file, unpack and copy the *folder* to your arduino sketches directory.

2: Install the SSD1306ASCII library (updated by Robin Edwards for speed)

3: Check your wiring corresponds to the pins in the comments/defines at the top of the eMBee_BASIC file.

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
LET variable = <expr> e.g. LET A$="Hello".
variable = <expr> e.g. A=5
LIST [start],[end] e.g. LIST or LIST 10,100
RUN [lineNumber]
GOTO lineNumber
REM <comment> e.g. REM ** My Program ***
STOP
CONT (continue from a STOP)
INPUT variable e.g. INPUT a$ or INPUT a(5,3)
IF <expr> THEN cmd e.g. IF a>10 THEN a = 0: GOTO 20
FOR variable = start TO end STEP step
NEXT variable
NEW
GOSUB lineNumber
RETURN
DIM variable(n1,n2...)
CLS
PAUSE milliseconds
POSITION x,y sets the cursor
PIN pinNum, value (0 = low, non-zero = high)
PINMODE pinNum, mode ( 0 = input, 1 = output)
LOAD (from internal EEPROM)
SAVE (to internal EEPROM) e.g. use SAVE + to set auto-run on boot flag
LOAD "filename", SAVE "filename, DIR, DELETE "filename" if using with external EEPROM.
```

"Pseudo-identifiers"
```
INKEY$ - returns (and eats) the last key pressed buffer (non-blocking). e.g. PRINT INKEY$
RND - random number betweeen 0 and 1. e.g. LET a = RND
```

Functions
```
LEN(string) e.g. PRINT LEN("Hello") -> 5
VAL(string) e.g. PRINT VAL("1+2")
INT(number) e.g. INT(1.5)-> 1
STR$(number) e.g. STR$(2) -> "2"
LEFT$(string,n)
RIGHT$(string,n)
MID$(string,start,n)
PINREAD(pin) - see Arduino digitalRead()
ANALOGRD(pin) - see Arduino analogRead()
```
