# rf-data-communication

RF Data Communication between two Personal Computers->

Personal computers are usually connected to each other through network cables in a LAN based office network. Connecting two computers using USB cable or RS – 232 cable for data communication is feasible option when the two systems are placed nearby. Also, a single network cable connects a PC to only another single PC. If computer systems in an office are made to communicate data wirelessly, the cost to install elaborate network cable wiring can be saved and the entire setup looks more neat and clean. This way any computer can be connected to any other computer without any fuss. A single computer can also be easily connected with any number of other computers at a time.

This project has illustrated wireless data communication between PCs using the 434 MHz RF module. The RF module has a range of 50-60 meter and can be extended to 300-350 meter using an antenna and increasing the transmission power of module. Therefore, the RF-based wireless data communication network can be installed to any small office or workplace. In the project, since PCs cannot directly interface with the RF module, they are interfaced through Arduino boards. The PC that has to work as data server is connected to an RF transmitter through the Arduino board while the PCs that have to work as data client in the wireless local network are connected to RF receivers through Arduino. The data communication has been illustrated using the serial monitor on both PCs.

In the project, one PC is configured to as data server and one another PC is configured to as data Client. Many PCs can be made data client but only one PC has been made client in the project for demonstration purpose. The PC made server is connected to an Arduino board using the USB cable and an RF transmitter is interfaced with the Arduino board for RF transmission. For interfacing the RF transmitter with Arduino, its serial input pin (Pin 2) is connected to pin 12 of the Arduino board and an antenna is attached to pin 4 of the RF transmitter module for range extension.

On the client PC, the PC is again connected to Arduino using the USB cable and The Arduino board is further interfaced to an RF receiver for a client like operation. For interfacing the RF receiver with the Arduino, the serial out pin (Pin 2) of the receiver module is connected to pin 11 of the client side Arduino board and an antenna is connected to pin 8 of the RF receiver for range extension. The VCC and ground are supplied at the respective pins of Arduino and RF modules as dictated by their datasheets.


How the Circuit Works?

In the project, data is transferred from the Server PC to the Client PC. The server PC transfers the data to be transmitted to the interfaced Arduino board through the USB cable. The serial monitor program is used to transfer data from PC to the Arduino board. The Arduino has the program code to read the character transferred to its buffer and serially transmit it to RF module using the VirtualWire Library functions. Since the RF module transmits only one character at a time, the character buffer received from the PC has to be stored in an array in the Server side Arduino program code.

On the client side PC, the character buffer transmitted over RF is detected by the RF receiver and passed serially to the client side Arduino board. The program code on the client side Arduino board reads the character buffer and store it in an array. The array elements are further serially transferred over USB cable to the client PC. At the Client PC, the received character array is displayed using the Serial Monitor program.


Programming Guide->


At the Server side Arduino, first, the VirtualWire library is imported to facilitate interfacing with RF module.

#include <VirtualWire.h>

An LED is connected at pin 13 to indicate serial transmission in progress. So, a variable “ledpin” is declared and assigned to pin 13 of the Arduino. The “MsgData” variable is declared to store the character reading and “MsgcharAR” array is declared to hold multiple characters before serial transmission. Some global variables – “length” to store the length of the message, “data_available” to check data availability and a counter “i” are initialized.

A setup() function is called, where, ledpin is set to output mode using the pinMode() function, the baud rate of the system is set to 9600 bits per second using the Serial.begin() function and vw_setup() function is used to set the baud rate of RF serial transmission to 2000 bits per second.

A loop() function is called, inside which, first length variable is initialized to 0 and serial data is checked if available using the Serial.available() function. If serial data is available, it is read using Serial.read() function and stored to MsgcharAR array. The length counter is incremented and data_available is set to 1  for Boolean logic. The code block is repeated in a While() loop until serial data is available.

If data is available, data_available flag variable is left to a Boolean setting of 1, then the message stored in MsgcharAR array is serially out to buffer using the Serial.print() function.

The message stored in the buffer has to be serially out on RF. The led indicating transmission in progress is switched ON by sending a HIGH logic to ledpin. The message is sent out serially on RF using the vw_send() function, where, message characters are first converted to unsigned character form. vw_wait_tx() is used to wait till the entire message is transmitted out. When the transmission is over, the ledpin is sent a LOW logic to switch the transmission in progress LED OFF.

The data_available is set to 0 and each element of MsgcharAR array is set to 0 as a default value.

data_available = 0;

This ends the server side Arduino Code.

On the client side Arduino, the program code first imports the required standard libraries. The VirtualWire library is imported to enable reception of serial data from the RF module.

The pin 13 where transmission progress indicator LED is connected is assigned to ledpin variable and two variables – “Sensor1Data” to capture the message in integer form and “Sensor1CharMsg” to store character representation of the message to be displayed are declared.

A setup() function is called where baud rate of the Arduino is set to 9600 bits per second using Serial.begin() function. The ledpin is set to output using the pinMode() function.

The RF transmitter and receiver module does not have Push To Talk pin. They go inactive when no data is present to transmit or receive respectively. Therefore  vw_set_ptt_inverted(true) is used to configure push to talk polarity and prompt the receiver to continue receiving data after fetching the first character. The baud rate for serial input is set to 2000 bits per second using vw_setup() function. The reception of the data is initiated using vw_rx_start().

A loop() function is called inside which, array “buf[]” to read serial buffer and “buflen” variable to store buffer length are declared.

The character buffer is detected using the vw_get_message() function, if it is present, a counter “i” is initialized and LED connected to pin 13 is switched on by sending a HIGH at pin 13 to indicate successful detection of the character  buffer.
The character buffer is stored in the Sensor1CharMsg array using the for loop with the initialized counter.

The data_available is set to 0 and each element of MsgcharAR array is set to 0 as default value.

This ends the client side Arduino code.

