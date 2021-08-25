---
title: ' Arduino IDE'
taxonomy:
    category:
        - docs
media_order: ontbrekende-driver-foutmelding-arduino.png
---

De Arduino IDE is een software pakket. (ook wel Integrated Development Enviroment genaamd.
Je moet beginnen met deze software voor jou besturingssysteem te downloaden via [arduino.cc](https://www.arduino.cc/en/software)
Deze software gebruik je om Arduino code (C++) te schrijven en te uploaden naar de Arduino NANO.

####Micro cursus Arduino IDE
##### Sketch
In de Arduino IDE heet een stuk code een 'sketch', een sketch heeft .ino als bestands extensie.
Een sketch is code gemaakt voor de Arduino, gebaseerd op de programmeer taal C++ met wat specifieke werk wijze.
#####Bibliotheken
De kracht van veel software pakketten is dat je gebruik kan maken van reeds door andere gemaakte bibliotheken. Deze bibliotheken kan je beheren via de bibliotheek manager via het menu 'Hulpmiddelen -> Bibliotheken beheren'. Dit is ook de makkelijkste methode om ontbrekende bibliotheken op te zoeken en te installeren.
#####Opbouw Sketch
De inhoud van de .ino (sketch) begint altijd met includes, zo voeg je functionaliteit van andere bibliotheken toe aan jouw programma.

    #include <Arduino.h>
    #include <arduino-timer.h>
    #include <SoftwareSerial.h>
    #include "config.h"

Zoals je kan zien zijn is de laatste include anders, dit is een lokale include te herkennen aan de dubbele quotes. Dit bestand is geen standaard geïnstalleerde bibliotheek, maar een bestand wat in dezelfde folder als de sketch staat.

Vervolgens zie je vaak een lijst met variabelen die in de code gebruikt worden. (waarbij //tekst commentaar van de ontwikkelaar is)

    // Variables
    bool isBraking = true;             // brake activated
    int speedCurrent = 0;              // current speed
    int speedCurrentAverage = 0;       // the average speed over last X readings

Vervolgens krijg je de setup( ) functie, deze word uitgevoerd bij het opstarten van de microcontroller.

    void setup() {
        Serial.begin(115200); SoftSerial.begin(115200); // Start reading SERIAL_PIN at 115200 baud
        TCCR1B = TCCR1B & 0b11111001; // TCCR1B = TIMER 1 (D9 and D10 on Nano) to 32 khz
        setThrottle(0);
    }
    
Gevolgd door hoofd functie genaamd loop( ). Zoals de naam doet vermoeden word deze code steeds herhaald. Dit is dan ook de functie die verantwoordelijk is voor de werking van het programma. Vanuit deze functie wordt eventueel andere code en bibliotheken aangesproken, maar dit is waar de 'magic' gebeurt.


    void loop() {
        readUART(); //read from step bus

        //speed reading from serial
        speedReadingsSum = speedReadingsSum - speedReadings[index];
        speedReadings[index] = speedCurrent;
        speedReadingsSum = speedReadingsSum + speedReadings[index];
        index++;
        ......
        
Dus in het kort heb je:
* includes die de bibliotheken inladen
* variabelen die in de code gebruikt worden en voor instellingen gebruikt kunnen worden
* een setup( ) functie die er voor zorgt dat alles goed opgestart word
* een hoofd loop( ) functie met de repeterende programma code

Met deze kennis kunnen we naar de broncode die we nodig hebben om van een _e-step_ een _step met ondersteuning_ te maken.

! Afhankelijk van je besturingsysteem moet je een driver installeren om met de NANO te communiceren. Een van de meest voorkomende chips is de CH430. Deze driver kan je [hier vinden](http://www.wch-ic.com/downloads/CH341SER_ZIP.html). Een foutmelding waaraan je een ontbrekende driver kan herkennen is: ![ontbrekende-driver-foutmelding-arduino](ontbrekende-driver-foutmelding-arduino.png "ontbrekende-driver-foutmelding-arduino")