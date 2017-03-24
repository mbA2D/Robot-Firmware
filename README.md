{\rtf1\ansi\ansicpg1252\cocoartf1404\cocoasubrtf460
{\fonttbl\f0\fswiss\fcharset0 Helvetica;}
{\colortbl;\red255\green255\blue255;}
\margl1440\margr1440\vieww11660\viewh16980\viewkind0
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural\partightenfactor0

\f0\fs24 \cf0 # Overview\
\
1. The operator (ie, remote control unit, the one with the wii nunchuck) sends packets to the robot via an Xbee (2.4GHz radio)\
\
- Packets are formatted using "Promulgate", check out below for the structure\
\
- It is all UDP, the Xbees are sending data without ACKs\
\
- This is in order to achieve a faster latency, we've been seeing an average of 150ms\
\
- Some messages can have a higher priority for inserting / replacing higher in the queue, see more below\
\
2. The robot receives the packets, then parses it to do an action\
\
- Safety: if no message has been received in some time, the robot brakes its motors\
\
3. Robot then sends back a message with sensor updates, iterating through a list of sensors - Operator receives the message\
\
4. Process repeats, continuing this exchange of messages\
\
\
# Packets\
\
- Here is how packets are formatted:\
\
char action, char cmd, uint8_t key, uint16_t val, char cmd2, uint8_t key2, uint16_t val2, char delim\
\
Action: Specifies the type of message. eg, '$' is used for iterated sensor data from robot\
\
Valid chars = '~', '@', '#', '^', '&', '$'\
\
Command (cmd & cmd2): Char representing the command, see below for API\
\
Valid chars = [A-Z], [a-z]\
\
Key (key & key2): 8 bit unsigned int representing a 'key' ... it's a number\
\
Val (val & val2): 16 bit unsigned int representing a value ... like an analog sensor\
\
Delim: Char representing the end of the command\
\
Valid chars = '!', ';', '?'\
\
\
# API\
\
OperatorSimple: Operator -> Robot\
------------------------------------------------\
L = left motor\
R = right motor\
S = arm\
C = claw\
P = red button\
G = green button\
Y = yellow button\
B = blue button\
W = white button\
\
BowieSimple: Robot -> Operator\
--------------------------------------------\
A = accelerometer\
O = gyroscope\
F = force sensors\
U = sonar sensors\
M = magnetometer\
H = altitude\
I = gpio\
T = temperature\
\
\
# Priority\
\
- All messages can be given a priority, however this number is not sent\
- Lower number = bigger priority\
\
BowieSimple: Robot -> Operator\
--------------------------------------------\
0 = none\
1 = force & sonar sensors\
2 = accel & gyro\
3 = gpio\
4 = magnetometer\
\
OperatorSimple: Operator -> Robot\
------------------------------------------------\
0 = none\
1 = button presses\
2 = servos\
3 = driving\
...\
9 = none\
\
\
\
\
\
\
}