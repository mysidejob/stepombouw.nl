---
title: 'KERS en COAST'
taxonomy:
    category:
        - docs
---

##Motorvermogen en motorrem
Om de step goed (en legaal) te laten werken moeten er een paar dingen aangepast worden aan de firmware en instellingen van de steps eigen software.
Deze software zit in de motorcontroller (ESC) en kan ingeladen worden via diverse tools (lijstje tools).

###Motorvermogen
Volgens de geldende regelgeving mag het **nominale vermogen** van de motor niet meer dan 250W zijn. Bij steppen met een hoger standaard vermogen kan je dit oplossen door de hoeveelheid stroom naar de motor te beperken.

> P = U * I , dus vermogen (P) is spanning (V) maal stroom (I). De spanning van de accu blijft gelijk, dus door de stroom terug te regelen neemt het vermogen van de motor af.

###KERS
KERS staat voor Kinetic Energy Recovery System. Vrij vertaald: **terugwinnen** van kinetische energie. 
Bij de originele werking van de step, remt de step direct af zodra je gas terug draait. Dit word gedaan met de motorrem, waardoor er weer energie terug de accu in loopt. Feitelijk draait de motor dan als dynamo. De KERS waarde kan je het beste op gemiddeld laten staan.

###COAST
In de werking van onze step willen we niet dat bij het loslaten van het gas de motorrem aan gaat, maar alleen bij het bedienen van de rem. Dus feitelijk wil je de step zonder tegenwerking van de motor laten uitrollen nadat de motorondersteuning uit staat. Dit heet in sommige tools COAST en in scooterhacking utility heet dit Disable KERS. 
Resultaat: Je remt alleen op de motor af zodra je remt.

> Binnen sommige software kan je instellen dat KERS alleen werkt boven een bepaalde snelheid, bijvoorbeeld boven de 25. Je step gaat dan remmen zodra je te snel gaat, bijvoorbeeld bij bergaf steppen.

! Bij het niet aanpassen van KERS / COAST zal je opmerken dat het lastig is om te versnellen en dat de step herkenning lastig werkt. De step remt immers te snel en teveel af.