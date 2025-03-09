# h7 Maalisuora

## Alustus

Harjoitukset a) - c) on tehty aiemmin kurssilla luodulla virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

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


## b) Lähdeviitteet

Tarkistin kaikkien raporttien lähdeviitteet 8.3.2025 ja lisäsin puuttuvat viittaukset.
 

## c) Uusi komento

Tarkoituksena oli luoda uusi komento niin, että kaikki käyttäjät voivat ajaa sitä. Aloitin tehtävän tekemisen 8.3.2025 klo 12.30.

Tarkastelin Shell Scripting -artikkelia (Karvinen 2007) ja päätin luoda samankaltaisen komennon. Loin ensin tiedoston nimeltä ````listaa````, ja lisäsin sinne komennot ````ls```` ja ````pwd```` lyhyiden lauseiden kera. Lisäsin tekstitiedoston alkuun ````#!/bin/bash````, jotta käyttöjärjestelmä osaa käyttää bashia komennon tulkitsemiseen (Geeks for Geeks 2022). Käytin komentoa ````sudo cp listaa /usr/local/bin/````, jotta tekemäni komento toimisi muissakin hakemistoissa (Karvinen 2007).

![kuva](https://github.com/user-attachments/assets/ad564aea-08ba-4d1a-bf23-ccc24f4858a5)

Tässä kohtaa luulin aluksi, että komento toimii kaikissa hakemistoissa, mutta myöhemmin huomasin, että olin unohtanut antaa ajo-oikeudet komennolle eikä se enää toiminut ollenkaan. Korjasin tilanteen.

![kuva](https://github.com/user-attachments/assets/9fd78b3c-0e40-4059-8684-ec66966d2b92)

Nyt komennon pystyi ajamaan omalla käyttäjällä sekä testikäyttäjällä.

![kuva](https://github.com/user-attachments/assets/5126ea3a-053d-4caa-a599-2e2b94dd7d2f)

![kuva](https://github.com/user-attachments/assets/7f627592-2200-486b-a2f0-7bfeabfc1bba)

Omalla käyttäjälläni komento toimi myös senhetkisessä hakemistossa komennolla ````./listaa````, mutta testikäyttäjällä ei, vaikka tiedostolla oli ajo-oikeudet.

![kuva](https://github.com/user-attachments/assets/0c369893-ca8a-43c9-8dcd-dc18151ff3de)

Polussa siis todennäköisesti on joku ongelma, mutta tämä jäi selvittämättä. Testaaja kuitenkin pystyi ajamaan komennon muodossa ````listaa````.

Aikaa kului tehtävään ja raportointiin 1h.


### Lähteet

Karvinen 2007, Shell Scripting: https://terokarvinen.com/2007/12/04/shell-scripting-4/. Luettu 8.3.2025.

Geeks for Geeks 2022, Shell Scripting – Define #!/bin/bash: https://www.geeksforgeeks.org/shell-scripting-define-bin-bash/. Luettu 9.3.2025.


## d) Vanha arvioitava labraharjoitus, kevät 2024

Tarkoituksena oli ratkaista vanha arvioitava labraharjoitus soveltuvin osin. Valitsin tähän kurssin tehtäväsivulta kevään 2024 harjoituksen, sillä ensimmäiset tehtävät näyttivät käsittelevän myös tällä kurssitoteutuksella käsiteltyjä asioita. Aloitin tehtävän tekemisen 9.3.2025 klo 8.15. 

### Alkutoimet

Ensin loin itselleni uuden tyhjän virtuaalikoneen ja asensin Debian 64-bitin samalla tavalla, kuin harjoituksessa h1 (Tatti 2025). Lisäsin myös tämän koneen virtuaalimuistia 128 megatavuun, jotta koneen toiminta nopeutui (Pasi S. 2025).

Loin kotihakemistoon kansion raporttia varten polkuun /home/liljaharj/report/index.md.

### b) Tiivistelmä

Vain oma käyttäjäni pystyy tarkastelemaan ja muokkaamaan raporttia, eli oikeudet on vain käyttäjällä read ja write.

Howdy toimii sellaisenaan kaikilla käyttäjillä kaikissa hakemistoissa, mutta testaaja-käyttäjällä ei muodossa ````./howdy````.

Koneen ip-osoite selaimessa avaa paikallisesti AI Kakosen nettisivun. Kotihakemistossa ei ole sivustoon liittyen rootin omistamia tiedostoja. Index-tiedoston muokkaamiseen ei tarvita sudoa.

Ssh-palvelin on asennettuna ja testikäyttäjän kirjautuminen localhostille onnistuu ssh:lla ilman salasanaa.

Kurssitoteutuksellamme ei käsitelty Djangoa, joten jätin kaksi viimeistä osiota tekemättä.


### c) Ei kolmea sekoseiskaa

![kuva](https://github.com/user-attachments/assets/d9f69142-6faf-4560-93c5-3357bb30e488)


### d) 'howdy'

![kuva](https://github.com/user-attachments/assets/430dc241-4382-4be9-8fb0-fe44e7939d2e)

![kuva](https://github.com/user-attachments/assets/ba83f60d-dc5c-4b7f-b472-bf4225259eaa)


### e) Etusivu uusiksi

![kuva](https://github.com/user-attachments/assets/e70a3727-dd53-47f3-9345-3ca42cbab746)

![kuva](https://github.com/user-attachments/assets/8ced9437-bdfe-4534-9bfd-3b18915e76af)

![kuva](https://github.com/user-attachments/assets/7ad1f04f-9c58-4149-bab8-8c2172bb44bc)


Oikeudet:

![kuva](https://github.com/user-attachments/assets/592f49e7-652e-4ab8-a115-fc14c8baf8fb)

![kuva](https://github.com/user-attachments/assets/a8e1b10b-88b3-42ad-9c27-818cdaef6e8b)

 
![kuva](https://github.com/user-attachments/assets/8ce88951-9edb-476b-9ec4-f05890c3a473)

![kuva](https://github.com/user-attachments/assets/55aa77c6-7195-4126-8d50-2d7445f97133)

![kuva](https://github.com/user-attachments/assets/ebcb7d73-15f1-4ced-a433-b68630542200)


### g) Salattua hallintaa


![kuva](https://github.com/user-attachments/assets/7968f78e-c220-4731-a9df-da1801143077)

![kuva](https://github.com/user-attachments/assets/de47fdcc-c579-407e-820e-2445fcaf56cd)


### h) Djangon lahjat ja h) Tuotantopropelli

Ei tehty.



### Lähteet

Karvinen 2024, Final Lab for Linux Palvelimet 2024 Spring: https://terokarvinen.com/2024/arvioitava-laboratorioharjoitus-2024-linux-palvelimet/. Luettu 9.3.2024.

Tatti 2025, h1 Oma Linux: https://github.com/liljahelm/linux-course/blob/b462973283c458d2c4de0f26776fffd783271c8e/h1-Oma-Linux.md. Luettu 9.3.2025.

Pasi S. 2025, h1 linux-asennus virtuaalikoneelle: https://github.com/PasiS1337/linux-course/blob/main/h1-linux-asennus-virtuaalikoneelle.md. Luettu 9.3.2025.

Linuxize 2019, How to Setup Passwordless SSH Login: https://linuxize.com/post/how-to-setup-passwordless-ssh-login/. Luettu 9.3.2025.

