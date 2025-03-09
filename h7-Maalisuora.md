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

Käytin komentoa ````sudo cp listaa /usr/local/bin/````, jotta tekemäni komento toimisi muissakin hakemistoissa (Karvinen 2007). Kokeilin komentoa hakemistossa Documents, ja se toimi.

![kuva](https://github.com/user-attachments/assets/9f3a1268-1dd2-4822-a90c-584c34804bf4)

Loin uuden testikäyttäjän komennolla ````sudo adduser````, ja testasin listaa-komentoa myös tällä. Komento toimi.

![kuva](https://github.com/user-attachments/assets/434c8d08-a38e-4052-8926-3a397717a936)

Muistin tässä kohtaa, etten muistanut antaa execute-oikeuksia tiedostolle, mutta siitä huolimatta se toimi.

Aikaa kului tehtävään ja raportointiin 30min.


### Lähteet

Karvinen 2007, Shell Scripting: https://terokarvinen.com/2007/12/04/shell-scripting-4/. Luettu 8.3.2025.


## Vanha arvioitava labraharjoitus, kevät 2024

Tarkoituksena oli ratkaista vanha arvioitava labraharjoitus soveltuvin osin. Valitsin tähän kurssin tehtäväsivulta kevään 2024 harjoituksen, sillä ensimmäiset tehtävät näyttivät käsittelevän myös tällä kurssitoteutuksella käsiteltyjä asioita. Aloitin tehtävän tekemisen 9.3.2025 klo 8.15. 

### Alkutoimet

Ensin loin itselleni uuden tyhjän virtuaalikoneen ja asensin Debian 64-bitin samalla tavalla, kuin harjoituksessa h1 (Tatti 2025). Lisäsin myös tämän koneen virtuaalimuistia 128 megatavuun, jotta resoluutio paranisi (Pasi S. 2025).

Loin kotihakemistoon kansion raporttia varten polkuun /home/liljaharj/report/index.md.

### b) Tiivistelmä


### c) Suojaa raportti Linux-oikeuksilla niin, että vain oma käyttäjäsi pystyy katselemaan raporttia

Poistin ryhmältä ja muilta luku-, kirjoitus- ja ajamisoikeudet kansiosta "report".

![kuva](https://github.com/user-attachments/assets/4cdc7ee1-5cc5-4a35-8b5c-d1125c9149e1)


### d) 'howdy'

![kuva](https://github.com/user-attachments/assets/35d19a4b-de8b-47fd-81a2-fb588ebb9380)

![kuva](https://github.com/user-attachments/assets/ba83f60d-dc5c-4b7f-b472-bf4225259eaa)

![kuva](https://github.com/user-attachments/assets/430dc241-4382-4be9-8fb0-fe44e7939d2e)



### e) Etusivu uusiksi

![kuva](https://github.com/user-attachments/assets/e70a3727-dd53-47f3-9345-3ca42cbab746)

![kuva](https://github.com/user-attachments/assets/8ced9437-bdfe-4534-9bfd-3b18915e76af)

![kuva](https://github.com/user-attachments/assets/7ad1f04f-9c58-4149-bab8-8c2172bb44bc)


oikeudet:

![kuva](https://github.com/user-attachments/assets/592f49e7-652e-4ab8-a115-fc14c8baf8fb)

![kuva](https://github.com/user-attachments/assets/a8e1b10b-88b3-42ad-9c27-818cdaef6e8b)


![kuva](https://github.com/user-attachments/assets/8ce88951-9edb-476b-9ec4-f05890c3a473)

![kuva](https://github.com/user-attachments/assets/55aa77c6-7195-4126-8d50-2d7445f97133)

![kuva](https://github.com/user-attachments/assets/ebcb7d73-15f1-4ced-a433-b68630542200)


Valmisteluineen 1h 45min



### g) Salattua hallintaa

### h) Djangon lahjat ja h) Tuotantopropelli

Kurssitoteutuksellamme ei käsitelty Djangoa, joten jätin nämä osiot tekemättä.





### Lähteet

Karvinen 2024, Final Lab for Linux Palvelimet 2024 Spring: https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/. Luettu 9.3.2024.

Tatti 2025, h1 Oma Linux: https://github.com/liljahelm/linux-course/blob/b462973283c458d2c4de0f26776fffd783271c8e/h1-Oma-Linux.md. Luettu 9.3.2025.

Pasi S. 2025, h1 linux-asennus virtuaalikoneelle: https://github.com/PasiS1337/linux-course/blob/main/h1-linux-asennus-virtuaalikoneelle.md



