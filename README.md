# OPTA-PROG-WATCHDOG-CLOUD
It is programmed in time to show how to arm the various data created in the Arduino Cloud even if the OPTA has lost connection to the internet or external power. Also, this tutorial will also be available on how to run a program without OPTING when it is offline from the internet.

# OR TUTORIAL COME HERE, INSIDE VOID SETUP

Inside void setup we need to add 4 lines of code to define and call some cloud functions:

ArduinoCloud.begin(ArduinoIoTPreferredConnection, false);

We define this line as "false" when we do not want it to reset the cloud watchdog, that is, not to reset the variables, so even if the OPTA is without external power or without an internet signal, the variables that were stored in the cloud will remain there after signal or power supply return.
To reset the variables you need to write a reset in the code.


ArduinoCloud.addCallback(ArduinoIoTCloudEvent::CONNECT, doThisOnConnect);
ArduinoCloud.addCallback(ArduinoIoTCloudEvent::SYNC, doThisOnSync);
ArduinoCloud.addCallback(ArduinoIoTCloudEvent::DISCONNECT, doThisOnDisconnect);

The 3 functions above are basically used to execute something when one of these three events are triggered.
In this case we have doThisOnConnect, doThisOnSync, doThisOnDisconnect event. So in these 3 events we can define that something happens when they are called.

![image](https://github.com/Engenharia-Finder/OPTA-PROG-WATCHDOG-CLOUD/assets/133161771/d5fd6988-ebd4-4a3f-b328-a68cc3d171ef)
