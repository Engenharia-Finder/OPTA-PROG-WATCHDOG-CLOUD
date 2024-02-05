# Overview
It is programmed in time to show how to arm the various data created in the Arduino Cloud even if the OPTA has lost connection to the internet or external power. Also, this tutorial will also be available on how to run a program without OPTING when it is offline from the internet.

# Goals 
Learn how to configure the Arduino Cloud Watchdog so that it does not reset saved variables and also perform programming if the OPTA loses communication with Wi-Fi

# OR TUTORIAL COME HERE, INSIDE VOID SETUP

Inside void setup we need to add 4 lines of code to define and call some cloud functions:
```
ArduinoCloud.begin(ArduinoIoTPreferredConnection, false);
```

We define this line as "false" when we do not want it to reset the cloud watchdog, that is, not to reset the variables, so even if the OPTA is without external power or without an internet signal, the variables that were stored in the cloud will remain there after signal or power supply return.
To reset the variables you need to write a reset in the code.

```
ArduinoCloud.addCallback(ArduinoIoTCloudEvent::CONNECT, doThisOnConnect);
ArduinoCloud.addCallback(ArduinoIoTCloudEvent::SYNC, doThisOnSync);
ArduinoCloud.addCallback(ArduinoIoTCloudEvent::DISCONNECT, doThisOnDisconnect);
```
The 3 functions above are basically used to execute something when one of these three events are triggered.
In this case we have doThisOnConnect, doThisOnSync, doThisOnDisconnect event. So in these 3 events we can define that something happens when they are called.

![image](https://github.com/Engenharia-Finder/OPTA-PROG-WATCHDOG-CLOUD/assets/133161771/d5fd6988-ebd4-4a3f-b328-a68cc3d171ef)

# INSIDE VOID LOOP
```
void loop() {
  ArduinoCloud.update();
  /*Your code will be executed here*/
}
```

Inside the void loop we will have the programming that will be executed whether connected or disconnected with the cloud, so we can, for example, place a count of activations of one of the OPTA outputs inside the loop.

![image](https://github.com/Engenharia-Finder/OPTA-PROG-WATCHDOG-CLOUD/assets/133161771/7d4b8738-d303-481e-bf8c-7d2530fd98b7)

# WITHIN THE REMAINING FUNCTIONS

```
void doThisOnConnect(){
  Serial.println("OPTA conectado a Arduino IoT Cloud");
}
```

Within the first function, we can execute code when OPTA is connected to the cloud.

```
void doThisOnSync(){
  Serial.println("Thing Properties sincronizado");
}
```

Inside the second function we can execute code when the / OPTA variables are synchronized with the cloud.

```
void doThisOnDisconnect(){
  Serial.println("OPTA está desconectado");
  disconnectAfter();
}
```

Inside the third function we have code that will call another function called "disconnectAfter", this third function will only be executed when OPTA loses its internet connection.

```
void disconnectAfter(){
  Serial.println("Entrando na rotina offline"); //Depois de desconectar
  while(ArduinoCloud.connected() == 0){         //Enquanto o conexão estiver em estado 0 (offline) ele executa o break para sair desse looping e executa o void loop com a programação em offline
    break;                                      
  }
}
```

The fourth function will only be called when the third function is triggered.
This fourth function is responsible for executing the code within the void loop offline while the ArduinoCloud was 0 (ZERO), while it is 0 it triggers the break to exit this function and goes to the void loop to execute the code offline.
If the OPTA has an internet signal, this function will not be executed and it will show the readings of the variables that were created in the cloud normally.

![image](https://github.com/Engenharia-Finder/OPTA-PROG-WATCHDOG-CLOUD/assets/133161771/9938b681-b8ad-4294-9e7d-35d358ec9a02)

