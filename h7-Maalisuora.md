# h7 Maalisuora

## Alustus

Harjoitukset a) ja c) on tehty aiemmin kurssilla luodulla virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

Virtuaalipalvelin on vuokrattu DigitalOcean-palvelusta, ja palvelimella on 1 Gt keskusmuistia ja 25 Gt:n SSD-levy.

Kaikki harjoitukset perustuvat tehtävänantoihin kevään 2025 Linux-palvelimet -kurssin sivustolla: https://terokarvinen.com/linux-palvelimet/.

## a) Hei maailma

Tarkoituksena oli kirjoittaa ja ajaa "Hei maailma" kolmella eri kielellä. Aloitin tehtävän 7.3.2025 klo 19.

### Python

Aloitin pythonista, koska se on minulle entuudestaan tutuin koodauskieli. Loin ensin tiedoston, jonka myöhemmin ajan. Käytin tekstieditorina microa, ja tiedostonimen loppuun tuli python-kielen vuoksi laittaa ````.py````-pääte (Linux Handbook). Kirjoitin tekstitiedostoon ````print("Hei maailma")````, tämän muistin ulkoa aiemmista opinnoista. Ajoin tiedoston oikealla komennolla ja sain tulosteen (Linux Handbook).

![image](https://github.com/user-attachments/assets/99295cd6-0b83-43f3-b08b-0c0107a17d92)

### JavaScript

asennus, pakettien päivitys

ajo nodella terminaalissa

![image](https://github.com/user-attachments/assets/8159be41-7f39-4aea-bb87-f133bba29ccc)

tiedoston luonti ja ajo tiedoston kautta

![image](https://github.com/user-attachments/assets/dfbda19a-39ca-4a1e-a588-38f50b751eab)

### Java

asennus, tiedoston luonti ja ajo

![image](https://github.com/user-attachments/assets/f915e8f3-52bb-4d0f-b4ce-f15e5c8e154f)



aikaa kulunut 30min



### Lähteet

Linux Handbook, Run Python Scripts in Linux Command Line: https://linuxhandbook.com/run-python/. Luettu 7.3.2025.

Geeks for Geeks, How do you Run JavaScript Through the Terminal? https://www.geeksforgeeks.org/how-to-compile-and-run-a-c-c-code-in-linux/. Luettu 7.3.2025.

Its Linux Foss, How to Install Node.js and npm on Debian 12 Linux: https://itslinuxfoss.com/install-node-js-npm-debian-12-linux/. Luettu 7.3.2025.

Linux for Devices, How to Run a Command-Line Java Program on Linux? https://www.linuxfordevices.com/tutorials/linux/run-command-line-java. Luettu 7.3.2025.
