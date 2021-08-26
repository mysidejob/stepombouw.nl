---
title: ' Arduino IDE'
taxonomy:
    category:
        - docs
media_order: ontbrekende-driver-foutmelding-arduino.png
---
####Micro cursus Arduino IDE
De Arduino IDE is een software pakket. (ook wel Integrated Development Enviroment genaamd.
Deze software gebruik je om Arduino code (C++) te schrijven en te uploaden naar de Arduino NANO.

#####Download Arduino IDE
Ga naar https://www.arduino.cc/en/software en download de Arduino IDE.

![download-arduino.png ]( download-arduino.png " download-arduino.png ")

Na het betalen van de vrijwillige $5 of, als je dat de vorige keer al gedaan hebt, even op de download link klikken.

![download-arduino-2.png ]( download-arduino-2.png " download-arduino-2.png ")

#####Install Arduino IDE
Installeer deze door er op te dubbelklikken.

![arduino-install.png ]( arduino-install.png " arduino-install.png ")

Bijna aan het einde vraagt hij nog of je extra drivers wil installeren.
Natuurlijk bevestig je dit.

![arduino-install-driver1.png ]( arduino-install-driver1.png " arduino-install-driver1.png ")

![arduino-install-driver2.png ]( arduino-install-driver2.png " arduino-install-driver2.png ")

Daarna is de installatie klaar.

![arduino-install-complete.png ]( arduino-install-complete.png " arduino-install-complete.png ")

#####Arduino Source code (Sketch) laden
Download de source code waarvan jij denkt dat het de beste is. Dit kan voor iedereen anders zijn.
Bereidt je er op voor dat je meerdere versies moet proberen, voordat je voor jouw gevoel de beste hebt. Alle versies zijn nog in ontwikkeling.

![download-github.png ]( download-github.png " download-github.png ")


Start arduino
Ga naar de boards manager en selecteer de arduino nano.
Dit is het board wat in je step past.

#####Arduino Board Setup
![arduino-setup-board.png ]( arduino-setup-board.png " arduino-setup-board.png ")

en sluit dan de arduino aan.
Pas dan kun je de serial port selecteren.

![arduino-load-ino-serialport.png ]( arduino-load-ino-serialport.png " arduino-load-ino-serialport.png ")

Laadt nu de source code van de ge-unzipte github. of

![arduino-load-ino.png ]( arduino-load-ino.png " arduino-load-ino.png ")
Arduino wil er wat extra's mee doen. Bevestig dit.

![arduino-load-ino-2.png ]( arduino-load-ino-2.png " arduino-load-ino-2.png ")

Druk op compile/Upload. Dit is het tweede icon van links.

![arduino-compile-and-upload.png ]( arduino-compile-and-upload.png " arduino-compile-and-upload.png ")
Daarna krijg je waarschijnlijk je eerste foutmelding. 

#####Arduino foutmeldingen
An error occurred while uploading the sketch

In mijn geval moest ik de oude bootloader pakken.
Dit zal voor de meeste personen zijn, die niet de 20 euro voor een originele arduino nano uitgegeven hebben.

![arduino-load-ino-old-bootloader.png ]( arduino-load-ino-old-bootloader.png " arduino-load-ino-old-bootloader.png ")

Een andere foutmelding kan zijn dat je de foute serial port hebt.

![arduino-setup-serialport.png ]( arduino-setup-serialport.png " arduino-setup-serialport.png ")

Probeer eens een andere.


Uiteindelijk gaat het lukken.

![arduino-load-ino-uploading.png ]( arduino-load-ino-uploading.png " arduino-load-ino-uploading.png ")


![arduino-load-ino-upload.png ]( arduino-load-ino-upload.png " arduino-load-ino-upload.png ")

#####Arduino finish
Success. Je eerste arduino is klaar. Nu even naar het volgende hoofstuk voor de inbouw.


##### Sketch
In de Arduino IDE heet een stuk code een 'sketch', een sketch heeft .ino als bestands extensie.
Een sketch is code gemaakt voor de Arduino, gebaseerd op de programmeer taal C++ met wat specifieke werk wijze.
#####Bibliotheken
De kracht van veel software pakketten is dat je gebruik kan maken van reeds door andere gemaakte bibliotheken. Deze bibliotheken kan je beheren via de bibliotheek manager via het menu 'Hulpmiddelen -> Bibliotheken beheren'. Dit is ook de makkelijkste methode om ontbrekende bibliotheken op te zoeken en te installeren.

Met deze kennis kunnen we naar de broncode die we nodig hebben om van een _e-step_ een _step met ondersteuning_ te maken.
#####Driver
! Afhankelijk van je besturingsysteem moet je een driver installeren om met de NANO te communiceren. Een van de meest voorkomende chips is de CH430. Deze driver kan je [hier vinden](http://www.wch-ic.com/downloads/CH341SER_ZIP.html). Een foutmelding waaraan je een ontbrekende driver kan herkennen is: ![ontbrekende-driver-foutmelding-arduino](ontbrekende-driver-foutmelding-arduino.png "ontbrekende-driver-foutmelding-arduino")

