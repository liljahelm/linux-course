# h2 Komentaja Pingviini

## x) Command Line Basics Revisited

- Komentoja siirtymiseen ja tarkasteluun: pwd, ls, cd, cd .., less
- Tiedostojen käsittely: nano, mkdir, mv, cp -r, rmdir, rm, rm -r
- SSH etähallinta: ssh + käyttäjänimi + palvelin, scp -r ... (kopioi turvallisesti kansion etälaitteeseen)
- Käyttöohjeet: man
- Muuta: tabulaattori x2 näyttää mahdollisuudet mitä kirjoittaa seuraavaksi, tabulaattori kerran täyttää komennon loppuun jos mahdollista (suositeltavaa käyttää mm. tiedostonimissä), history näyttää ajetut komennot
- Tärkeitä hakemistoja: / (root), /home/ (kaikkien käyttäjien kotihakemistot), /home/username/ (käyttäjän kotihakemisto, ainoa paikka missä käyttäjä voi pysyvästi säilyttää tietoa), /etc/ (kaikki järjestelmän asetukset, tekstinä), /media/ (poistettavissa oleva media), /var/log/ (järjestelmän laajuiset lokit)
- Admin-komennot: sudo
- Pakettien hallinta: apt-get update jne.

(Lähde: Karvinen 2020, Command Line Basics Revisited: https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited)


## Alustus

Seuraavat harjoitukset on tehty edellisessä harjoituksessa luodulla virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia käytössä 4000 Mt. Edellisen oppitunnin vinkin myötä lisäsin virtuaalikoneen virtual memoryä 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB). 


## a) Micro, 25.1.2025 klo 17.20

Aluksi avasin virtuaalikoneen terminaalin työpöydän Applications-painikkeen kautta. Aloitin micro-editorin asentamisen päivittämällä listan saatavilla olevista paketeista komennolla sudo apt-get update. Asensin micron komennolla sudo apt-get -y install micro, ja asennus onnistui.

![kuva](https://github.com/user-attachments/assets/1e20fd36-94ca-421b-a2e8-13f73faeb179)



## b) Apt, 25.1.2025 klo 17.27

### Nano

En ole aikaisemmin käyttänyt Linuxia, joten myös kaikki komentoriviohjelmat olivat minulle uusia. Aluksi halusin asentaa jonkin yksinkertaisen ohjelman yksittäisenä, ja valitsin tekstieditori nanon. Asensin sen komennolla sudo apt-get -y install nano. Avasin editorin komennolla nano TESTI.TXT, ja kirjoitin tekstiä tiedostoon. Tallensin ja poistuin editorista näppäinyhdistelmällä Ctrl + X. 

![kuva](https://github.com/user-attachments/assets/e33118d5-4d43-4f05-a3f2-efd068aa3977)


### Htop ja tree

Seuraavat ohjelmat päätin yrittää asentaa yhdellä komennolla. Koska Linuxin käyttö ei ole tuttua, oli haastavaa miettiä, mikä voisi olla hyödyllinen ohjelma asentaa. Selasin eri ohjelmia linux.fi-sivustolla (https://www.linux.fi/wiki/Luokka:Komentorivin_perusty%C3%B6kalut) ja päätin asentaa ohjelmat Tree ja htop. Käytin komentoa sudo apt-get -y install htop Tree, mikä ei toiminut. Olin virheellisesti kirjoittanut treen isolla alkukirjaimella, ja korjauksen jälkeen asennus onnistui.

Tree (komento tree) näytti hakemistojen sisällöt puumaisessa muodossa, ja komentoon voi lisätä tarkennuksia eri kirjaimilla, esimerkiksi -d tulosti pelkät hakemistot. (Lähde: linux.fi, https://www.linux.fi/wiki/Tree). Tree vaikuttaa aloittelijan silmin hyödylliseltä apuvälineeltä visualisoimaan tiedostojärjestelmän rakennetta. 


![kuva](https://github.com/user-attachments/assets/787e62cd-1e7a-4854-8689-00f333d10ebe)


![kuva](https://github.com/user-attachments/assets/b9f193dd-afe0-4033-af25-66a18267689b)


Htop-ohjelmaa käytetään tarkkailemaan ja hallitsemaan interaktiivisesti tietokoneen prosesseja. (Lähde: linux-fi,  https://www.linux.fi/wiki/Htop).
Käynnistin ohjelman komennolla htop, jonka jälkeen ohjelma avautui. Näkymän alalaidassa oli lueteltuna mahdollisia toimintoja, joita voisi käyttää. Vahingossa huomasin, että ohjelma toimii myös hiirellä, ja toimintojen valitseminen onnistui myös klikkaamalla. En juurikaan osannut tulkita tai hyödyntää ohjelman näyttämiä tietoja, joten päädyin käyttökokeilussani vain muuttamaan ulkoasun väriä.


![kuva](https://github.com/user-attachments/assets/42d30c5d-dc99-473d-9130-7daae804eacd)


![kuva](https://github.com/user-attachments/assets/2547c24d-d8ad-408f-8861-4f78abfd773a)



## c) FHS


## d) The Friendly M


## e) Pipe


## f) Rauta


#### g) Vapaaehtoinen: Valitse muutama rivi lokeista. Tulkitse ja analysoi.
#### h) Vapaaehtoinen: Asenna jokin plugin micro-editorille ja kokeile sitä. Vaikkapa palettero, cheat tai runit.

