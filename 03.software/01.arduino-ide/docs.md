---
title: 'Arduino IDE'
taxonomy:
    category:
        - docs
---

Voor het installeren van de juiste firmware op de arduino heb je de arduino IDE nodig.

Hiermee compileer je de source code van Github en maak je er firmware voor de arduino nano van.

Ga naar https://www.arduino.cc/en/software en download de Arduino IDE.

![download-arduino.png ]( download-arduino.png " download-arduino.png ")

Na het betalen van de vrijwillige $5 of, als je dat de vorige keer al gedaan hebt, even op de download link klikken.

![download-arduino-2.png ]( download-arduino-2.png " download-arduino-2.png ")

Installeer deze door er op te dubbelklikken.

![arduino-install.png ]( arduino-install.png " arduino-install.png ")

Bijna aan het einde vraagt hij nog of je extra drivers wil installeren.
Natuurlijk bevestig je dit.

![arduino-install-driver1.png ]( arduino-install-driver1.png " arduino-install-driver1.png ")

![arduino-install-driver2.png ]( arduino-install-driver2.png " arduino-install-driver2.png ")

Daarna is de installatie klaar.

![arduino-install-complete.png ]( arduino-install-complete.png " arduino-install-complete.png ")


Download de source code waarvan jij denkt dat het de beste is. Dit kan voor iedereen anders zijn.
Bereidt je er op voor dat je meerdere versies moet proberen, voordat je voor jouw gevoel de beste hebt. Alle versies zijn nog in ontwikkeling.

![download-github.png ]( download-github.png " download-github.png ")


Start arduino
Ga naar de boards manager en selecteer de arduino nano.
Dit is het board wat in je step past.

![arduino-setup-board.png ]( arduino-setup-board.png " arduino-setup-board.png ")

en sluit dan de arduino aan.
Pas dan kun je de serial port selecteren.

![arduino-load-ino-serialport.png ]( arduino-load-ino-serialport.png " arduino-load-ino-serialport.png ")

Laadt nu de source code van de ge-unzipte github.

![arduino-load-ino.png ]( arduino-load-ino.png " arduino-load-ino.png ")
Arduino wil er wat extra's mee doen. Bevestig dit.

![arduino-load-ino-2.png ]( arduino-load-ino-2.png " arduino-load-ino-2.png ")

Druk op compile

![arduino-compile-and-upload.png ]( arduino-compile-and-upload.png " arduino-compile-and-upload.png ")
Daarna krijg je waarschijnlijk je eerste foutmelding. 

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

Success. Je eerste arduino is klaar. Nu even naar het volgende hoofstuk voor de inbouw.


















