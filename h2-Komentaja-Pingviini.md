# h2 Komentaja Pingviini

## x) Command Line Basics Revisited

- Komentoja siirtymiseen ja tarkasteluun: pwd, ls, cd, cd .., less
- Tiedostojen käsittely: nano, mkdir, mv, cp -r, rmdir, rm, rm -r
- SSH etähallinta: ssh + käyttäjänimi + palvelin, scp -r ... (kopioi turvallisesti kansion etälaitteeseen)
- Käyttöohjeet: man
- Muuta: tabulaattori x2 näyttää mahdollisuudet mitä kirjoittaa seuraavaksi, tabulaattori kerran täyttää komennon loppuun jos mahdollista (suositeltavaa käyttää mm. tiedostonimissä), history näyttää ajetut komennot
- Tärkeitä hakemistoja: /, /home/, /home/username/, /etc/, /media/, /var/log/
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



## c) FHS, 24.1.2025 klo 18.15

### Tärkeitä hakemistoja:

- /home/username/: Käyttäjän oma kotihakemisto, ainoa paikka missä käyttäjä voi pysyvästi säilyttää tietoa. Omalla kohdallani hakemisto on /home/liljat/, ja virtuaalikoneen terminaali avaa joka kerta oletuksena tämän hakemiston. Komento ls näyttää hakemiston sisällön. Komennolla cd Documents/ tarkastelin kyseisen kansion sisältöä, ja siellä oli aiemmin luomani tekstitiedosto TESTI.TXT, jonka olin siirtänyt sinne komennolla mv TESTI.TXT Documents/.

![image](https://github.com/user-attachments/assets/da1f9e6c-f923-45d3-8d20-67c821bc2f4f)


- /home/: Sisältää kaikkien käyttäjien kotihakemistot. Tähän hakemistoon pääsin omasta kotihakemistostani komennolla cd .., ja hakemiston sisällön näin komennolla ls. Hakemisto sisälsi vain oman hakemistoni (liljat), koska muita käyttäjiä ei ole luotu.

![image](https://github.com/user-attachments/assets/59f97b5c-da28-4081-87a3-cb5210a5f193)


- /: Root eli juurihakemisto on tiedostojärjestelmän ylin taso. /home/-hakemistosta pääsin root-hakemistoon tutulla komennolla cd .., ja sisältö näkyi komennolla ls. Juurihakemistossa näkyi jo valmiina seuraavat kolme tärkeää hakemistoa.

![image](https://github.com/user-attachments/assets/1eb6dcfd-7773-4f7d-bec9-00cd8abf0744)


- /etc/: Kaikki järjestelmän laajuiset asetukset tekstitiedostoina. Juurihakemistosta tänne pääsi komennolla cd etc.

![image](https://github.com/user-attachments/assets/20015f6b-1732-4e35-a1d9-13c7097b7952)

Valitsin kohdan aliases havainnollistamisen kohteeksi. Yritin ensin avata tätä komennolla cd aliases, mutta kyseessä olikin tekstitiedosto, ja sitä pääsi tarkastelemaan komennolla less aliases.

![image](https://github.com/user-attachments/assets/71b8b40b-2c79-4d54-8098-42ac942fd3cd)


![image](https://github.com/user-attachments/assets/f2263ab6-0b7e-4858-8671-36c77a382f14)


- /media/: Poistettavissa oleva media. Juurihakemistosta pääsi tänne komennolla cd media. Hakemistossa ei ollut mitään, koska mitään mediaa ei ole lisätty.

![image](https://github.com/user-attachments/assets/8ac8e3b1-31a3-4ceb-8a36-5ef3bd179cb8)

- /var/log/: Järjestelmän laajuiset lokit. Komento cd /var/log/ vei suoraan hakemistoon.

![image](https://github.com/user-attachments/assets/43e771a8-5a3b-4c55-8a0f-11fb1eeb9b3a)

Kokeilin, mitä hakemisto cups sisältää komennolla cd cups. Sisältä löytyi ainakin tekstitiedostoja, joista kokeilin avata ensimmäistä komennolla less access_log. Tiedosto ei auennut, ja terminaali ilmoitti ettei tähän ole oikeuksia. Päätin kokeilla samaa komentoa alkusanalla sudo, koska tiesin että se suorittaa komennon adminina, ja tämä onnistui.

![image](https://github.com/user-attachments/assets/4aaf4dd6-52ab-4040-876c-d74a7f9b1ee2)
![image](https://github.com/user-attachments/assets/08525b43-8b7b-4b8d-8af8-fbb4cce7d408)

![image](https://github.com/user-attachments/assets/eba9d1cf-769f-47d1-ad4d-cf66c02ff9e6)




(Lähde: Karvinen 2020, Command Line Basics Revisited: https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited)


## d) The Friendly M, 25.1.2025 klo 13.57

Tutustuin aluksi grep-komennon manuaaliin komennolla man grep. Ensimmäiseksi kokeilin komentoa grep -r, jonka pitäisi käydä läpi kaikki tiedostot kaikista hakemistoista. Syötin komennoksi grep -r tekstiä, koska tiesin, että sanan "tekstiä" pitäisi löytyä aiemmin luomastani tekstitiedostosta. Tämä onnistui.

![kuva](https://github.com/user-attachments/assets/b887d7f8-da10-4d38-8fd9-27506c77ea07)

Toiseksi kokeilin grep -c -komentoa, jonka pitäisi laskea, montako riviä täsmää hakusanaan. Aluksi komento ei toiminut, koska en ollut terminaalissa siinä hakemistossa, missä tekstitiedosto sijaitsi. Kun siirryin Documents-hakemistoon, komento grep -c 'tekstiä' TESTI.TXT toimi, ja palautti numeron 1. Hakusana ilmenee siis tiedostossa yhdellä rivillä.

![kuva](https://github.com/user-attachments/assets/d8532439-9020-4e80-b36e-00d084904998)


## e) Pipe, 25.1.2025 klo 14.17

Putkittamisessa komennot yhdistetään toisiinsa merkillä |. En suoraan osannut itse keksiä esimerkkiä putkittamisesta, joten hain apua netistä. FreeCodeCampin sivustolla oli listattu esimerkkejä putkittamisesta, joista kokeilin ensimmäistä, eli ls -l | wc -l. Alkuosa listaa hakemiston sisällön pitkässä muodossa, ja kun se putkitetaan jälkimmäiseen osioon, se laskee rivien määrän hakemistossa. Omassa juurihakemistossani on tämän perusteella 25 kohdetta.

![kuva](https://github.com/user-attachments/assets/19863ac4-416f-420c-983b-6e4b35415b38)

(Lähde: freeCodeCamp, How to Use Piping and Redirection in the Linux Terminal: https://www.freecodecamp.org/news/linux-terminal-piping-and-redirection-guide/)


## f) Rauta, 25.1.2025 klo 14.36

Asensin ensin lshw:n komennolla sudo apt-get install -y lshw. Tämän jälkeen ajoin komennon sudo lshw -short -sanitize saadakseni listauksen koneen raudasta.

![kuva](https://github.com/user-attachments/assets/a20e873a-cedd-4933-ba10-4f3871758ce4)

Huomioni kiinnittyi ensimmäisenä prosessoriin, joka on sama kuin omassa tietokoneessani. Muistelen, että virtuaalikone käyttää virtualisoinnista huolimatta fyysisen koneen resursseja, joten havainto selittynee tällä. Kuitenkin monessa kohdassa listausta lukee myös vain "VirtualBox", ja oletan, että VirtalBox-ohjelma itsessään huolehtii näistä resursseista. Muistia näytti olevan 4 GB, eli saman verran mitä asennuksen yhteydessä määrittelin. 

Tietokoneiden fyysiset ominaisuudet tarkemmin ovat itselleni vielä aika vieraita, joten en aluksi keksinyt muuta huomioitavaa edellä todetun lisäksi. Hain netistä tietoa lshw-komennosta. Täsmennys -short tarkoittaa lähteeni mukaan laitteistojen polkua, -sanitize poistaa arkaluontoiset tiedot näkymästä. (Lähde: https://www.geeksforgeeks.org/lshw-command-in-linux-with-examples/).

Listauksessa on eritelty tiedot laitteisiin, luokkiin ja kuvaukseen. Laite eli device ilmeisesti kertoo, mitä fyysisen laitteiston osaa tieto koskee, tästä en löytänyt netistä tarkempaa määritelmää. Class kertoo tiedon tyypin, esimerkiksi liittyykö se muistiin, prosessoriin vai verkkolaitteisiin. Kuvaukseen on määritelty tarkemmin laitteisto. Lisäksi listauksen vasen laita kertoo laitteiston polun. 

Löytämäni tieto internetissä käsitteli pääasiassa, mihin komentoa käytetään ja miksi, mutta en löytänyt sellaista tietoa, joka olisi auttanut minua analysoimaan listausta paremmin. Fyysiseen laitteistoon liittyvät artikkelit mm. esittelivät käsitteitä, joihin olenkin jo tutustunut. Kuitenkin ymmärsin sen, että komennon ja siihen liitettävien eri täsmennyksien avulla voidaan saada hyvinkin yksityiskohtaista tietoa laitteistosta, kun sitä tarvitaan.

