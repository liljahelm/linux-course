# h7 Maalisuora

## Alustus

Harjoitukset a) ja c) on tehty aiemmin kurssilla luodulla virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

Virtuaalipalvelin on vuokrattu DigitalOcean-palvelusta, ja palvelimella on 1 Gt keskusmuistia ja 25 Gt:n SSD-levy.

Kaikki harjoitukset perustuvat tehtävänantoihin kevään 2025 Linux-palvelimet -kurssin sivustolla: https://terokarvinen.com/linux-palvelimet/.

## a) Hei maailma

Tarkoituksena oli kirjoittaa ja ajaa "Hei maailma" kolmella eri kielellä. Aloitin tehtävän 7.3.2025 klo 19.

### Python

Aloitin pythonista, koska se on minulle entuudestaan tutuin koodauskieli. Loin ensin tiedoston, jonka myöhemmin ajan. Käytin tekstieditorina microa, ja tiedostonimen loppuun tuli python-kielen vuoksi laittaa ````.py````-pääte (Linux Handbook). Kirjoitin tekstitiedostoon ````print("Hei maailma")````, tämän muistin ulkoa aiemmista opinnoista. Ajoin tiedoston oikealla komennolla ja sain tulosteen (Linux Handbook). En joutunut asentamaan pythonia tässä kohtaa erikseen. Voi olla, että olin tehnyt sen jo oppitunnin aikana.

![kuva](https://github.com/user-attachments/assets/ca8e7bd7-a2d7-41b5-ab8f-b1b9e483bf54)


### Java

Asensin Javan komennolla ````sudo apt install default-jdk````. Loin tiedoston micro-editorilla, johon lisäsin koodin, ja ajoin tiedoston onnistuneesti. (Linux for Devices.)

![kuva](https://github.com/user-attachments/assets/ee802179-0612-4db9-b992-e26ca4403e5f)


### Bash

Loin jälleen uuden tiedoston micro-editorilla, lisäsin koodin ja ajoin tiedoston (Karvinen 2018).

![kuva](https://github.com/user-attachments/assets/f5fa6266-9b1c-4eab-9d05-a60ea24a47af)


Aikaa tähän tehtävään ja raportointiin kului 40min.

### Lähteet

Linux Handbook, Run Python Scripts in Linux Command Line: https://linuxhandbook.com/run-python/. Luettu 7.3.2025.

Linux for Devices, How to Run a Command-Line Java Program on Linux? https://www.linuxfordevices.com/tutorials/linux/run-command-line-java. Luettu 7.3.2025.

Karvinen 2018, Hello World Python3, Bash, C, C++, Go, Lua, Ruby, Java – Programming Languages on Ubuntu 18.04: https://terokarvinen.com/2018/hello-python3-bash-c-c-go-lua-ruby-java-programming-languages-on-ubuntu-18-04/. Luettu 7.3.2025.


## Lähdeviitteet

Tarkistin kaikkien raporttien lähdeviitteet 8.3.2025 ja lisäsin muutamaan ensimmäiseen raporttiin maininnan kurssin sivustosta.
 

## Uusi komento

Tarkoituksena oli luoda uusi komento niin, että kaikki käyttäjät voivat ajaa sitä. Aloitin tehtävän tekemisen 8.3.2025 klo 12.30.

Tarkastelin Shell Scripting -artikkelia (Karvinen 2007) ja päätin luoda samankaltaisen komennon. Loin ensin tiedoston nimeltä ````listaa````, ja lisäsin sinne komennot ````ls```` ja ````pwd```` lyhyiden lauseiden kera.

![kuva](https://github.com/user-attachments/assets/a7696d36-715e-488e-8014-2168f37e84c2)

Komento toimi samassa hakemistossa ollessa, mutta ei muuten.

![kuva](https://github.com/user-attachments/assets/6f8cf380-83ad-4249-ba9d-3d6e92003e06)

Käytin komentoa ````sudo cp lspwd /usr/local/bin/````, jotta tekemäni komento toimisi muissakin hakemistoissa (Karvinen 2007). Kokeilin komentoa hakemistossa Documents, ja se toimi.

![kuva](https://github.com/user-attachments/assets/9f3a1268-1dd2-4822-a90c-584c34804bf4)

Loin uuden testikäyttäjän komennolla ````sudo adduser````, ja testasin listaa-komentoa myös tällä. Komento toimi.

![kuva](https://github.com/user-attachments/assets/434c8d08-a38e-4052-8926-3a397717a936)

Muistin tässä kohtaa, etten muistanut antaa execute-oikeuksia tiedostolle, mutta siitä huolimatta se toimi.

Aikaa kului tehtävään ja raportointiin 30min.



### Lähteet

Karvinen 2007, Shell Scripting: https://terokarvinen.com/2007/12/04/shell-scripting-4/. Luettu 8.3.2025.

