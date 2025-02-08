# h3 Hello Web Server

## x) Tiivistelmät

### Teoriasta käytäntöön pilvipalvelimen avulla

a) Pilvipalvelimen vuokraus ja asennus
- Monet eri alustat tarjoavat kuukausihintaan vuokrattavia virtuaalipalvelimia eli "droplets" eri laajuuksissa ja eri hintaan
- Asennusta varten valitaan oikea käyttöjärjestelmä, haluttu palvelun laajuus, virtuaalikoneen ominaisuudet, sijainti, autentikointimenetelmä ja koneiden määrä
- Domainnimi vuokrataan ja ohjataan se osoittamaan omalle virtuaalipalvelimelle

d) Palvelin suojaan palomuurilla
- Palomuuri asennetaan komennolla "sudo apt-get install ufw"
- Palomuuriin tehdään reikä porttia varten komennolla "sudo ufw allow 22/tpc"
- Palomuuri käynnistetään komennolla "sudo ufw enable"

e) Kotisivut palvelimelle
- Ensin luodaan virtuaalipalvelimelle käyttäjä komennolla "sudo adduser" ja tehdään siitä pääkäyttäjä
- SSH yhteys avataan ja tieto saatavilla olevista päivityksistä päivitetään
- Juuri lukitaan ja virtuaalikoneelle asennetaan tietoturvapäivitykset
- Virtuaalikoneelle ladataan Apache-palvelin
- Palomuuriin tehdään toinen reikä porttia varten
- Apachen testisivu korvataan omalla HTML-sivulla

f) Palvelimen ohjelmien päivitys
- SSH-yhteys avataan virtuaalipalvelimen pääkäyttäjän tunnuksilla
- Haetaan tiedot saatavista päivityksistä sekä asennetaan ne, ml. tietoturvapäivitykset

### Lähteet
Lehto 2022, Teoriasta käytäntöön pilvipalvelimen avulla (h4): https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/


### First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS
- Uuden palvelimen luomisen yhteydessä valitaan datakeskus lähellä asiakkaiden sijaintia
- Ensimmäinen kirjautuminen tehdään root-käyttäjänä komennolla "ssh root@10.0.0.1", malli-IP-osoitteen tilalle oma IP-osoite
- Vahvat salasanat ovat tärkeitä
- Palomuuriin tulee tehdä reikä SSH:ta varten ennen sen käyttöönottoa
- Koneelle luodaan uusi käyttäjä ja sitä testataan uudessa paikallisessa terminaalissa ennen, kuin suljetaan viimeisin shell etähostilla
- Root-käyttäjä lukitaan ja SSH-kirjautuminen sillä estetään
- Ohjelmistot pitää päivittää tietoturvan vuoksi
- Julkista palvelinta varten tulee tehdä reikä palomuuriin esim. komennolla "sudo ufw allow 80/tcp"

### Lähteet

Karvinen 2017, First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS: 
https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/


## Alustus

Seuraavat harjoitukset on tehty virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

Kaikki harjoitukset perustuvat tehtävänantoihin kevään 2025 Linux-palvelimet -kurssin sivustolla: https://terokarvinen.com/linux-palvelimet/.

## a) Virtuaalipalvelimen vuokraus DigitalOceanista, 8.2.2025 klo 11.45

Olin jo aikaisemmin rekisteröitynyt GitHub Education -palveluun saadakseni ilmaisia krediittejä DigitalOcean-palveluun, josta voisin vuokrata virtuaalipalvelimen. GitHub-tunnuksilla pystyin rekisteröitymään DigitalOceaniin, ja sain 200 dollarin edestä krediittejä tätä tarkoitusta varten.

Avasin DigitalOceanin virtuaalikoneeni verkkoselaimella. Kirjauduttuani sisään näkymänä oli heti työpöytä otsikolla "first-project". Aloitin palvelimen vuokraamisen valitsemalla yläpalkista "Create" ja sieltä "Droplets".

![kuva](https://github.com/user-attachments/assets/2d465e1c-bc82-4b23-80e5-fef62666e38e)

Annetuista vaihtoehdoista lähin datakeskus Helsingistä katsottuna on Amsterdam, joten valitsin sen.

![kuva](https://github.com/user-attachments/assets/1b49b7a4-731c-43c2-a004-7365616081d7)

Virtuaalikoneeni käyttöjärjestelmä on Debian 12 64-bit, joten valitsin käyttöjärjestelmäksi myös tässä.

![kuva](https://github.com/user-attachments/assets/4b5c7bf3-fd5e-4b22-b90f-812b6e2b4a1e)

Jätin "Droplet type"-kohtaan valinnan basic, ja kuvauksenkin mukaan sen pitäisi sopia hyvin tämän harjoituksen kaltaista pientä projektia varten. Valitsin prosessoriksi vaihtoehdon "Regular" ja kuuden dollarin paketin, joka sisälsi 1 Gt keskusmuistia. Tätä suositeltiin edellisen oppitunnin aikana. 

![kuva](https://github.com/user-attachments/assets/89201516-5590-464a-97ff-fd847cbfd635)

Palvelu tarjosi ylimääräistä tallennustilaa ja automaattisia varmuuskopioita, mutta kurssin ohjeistuksen mukaisesti näille ei pitäisi olla tarvetta, eli en valinnut niitä.

Varmennustavaksi valitsin SSH:n, sillä edellisen oppitunnin perusteella se on salasanaa tietoturvallisempi vaihtoehto, ja sen käyttöönotto vaikutti ymmärrettävältä. Valitsin kohdan "Add SSH Key", ja palvelu antoi ohjeet ja tarvittavat komennot avaimien luomista varten. 

Avasin virtuaalikoneen terminaalin ja kokeilin komentoa "ssh-keygen", mutta sitä ei löytynyt. Ensin piti siis asentaa tämä komennolla "sudo apt-get install openssh-client", jota edeltävästi hain saatavilla olevat päivitykset komennolla "sudo apt-get update". Tämän jälkeen tehtävänannon ohjeen mukaisesti painoin tulevissa valinnoissa kolme kertaa enteriä. Asennus onnistui. Syötin vielä komennon "micro $HOME/.ssh/id_rsa.pub", joka avasi tekstieditorin, josta sain kopioitua julkisen avaimen ja liitettyä DigitalOceaniin. Annoin julkiselle avaimelle lopuksi nimen.

![kuva](https://github.com/user-attachments/assets/190c53dd-c892-47c9-acd2-2416e77cf1d1)

Seuraavaksi DigitalOcean tarjosi lisää lisäpalveluita, joita en valinnut. Lopuksi jätin valinnat oletuksiksi, eli vain yksi virtuaalipalvelin, enkä muokannut sen nimeä. Loin dropletin klikkaamalla "Create Droplet".

![kuva](https://github.com/user-attachments/assets/dc1bf166-47af-4dad-a9a4-6bcdf950f46b)

Asennus tapahtui nopeasti, ja sen päätteeksi sain myös palvelimen IP-osoitteen.

![kuva](https://github.com/user-attachments/assets/2332735d-6a77-46f5-9b14-ac3dacd09709)


## b) Alkutoimet virtuaalipalvelimella, 8.2.2025 klo 12.20-13.18

### Kirjautuminen

Kirjauduin virtuaalipalvelimelle root-käyttäjänä onnistuneesti komennolla "ssh root@178.62.233.5". Terminaaliin ilmestyi kysymys, joka varmisti, että yhteys halutaan muodostaa, ja vastasin tähän "yes". Tämän jälkeen kirjautuminen onnistui ja yhteys avautui. 

![kuva](https://github.com/user-attachments/assets/7bf67942-0e28-4891-b81b-b79e35034d61)

### Palomuurin asentaminen

Ensin hain saatavilla olevat päivitykset komennolla "sudo apt-get update". Asensin palomuurin komennolla "sudo apt-get install uwf", tein siihen reiän komennolla "sudo ufw allow 22/tcp" ja käynnistin komennolla "sudo ufw enable". Terminaali varmisti operaation jatkamisen, johon vastasin myöntävästi, ja sain tekstin että palomuuri on aktiivinen ja käynnistyy järjestelmän käynnistyessä.

![kuva](https://github.com/user-attachments/assets/3b04e741-0386-4ca6-9924-1a635b8af78f)  

### Käyttäjän luominen

Lisäsin itselleni sudo-käyttäjän komennolla "sudo adduser liljat", jonka jälkeen annoin vahvan salasanan käyttäjälle sekä täytin tietoihin etunimen ja sukunimen. Muihin kysyttyihin tietoihin en täyttänyt mitään. 

![kuva](https://github.com/user-attachments/assets/669d63a6-eacd-48bb-b6ef-e598a112df21)

Viimeistelin vaiheen komennolla "sudo adduser liljat sudo". 

![kuva](https://github.com/user-attachments/assets/65b13cd6-3b15-4ba2-baf0-999874c7f216)

Kopioin root:n ssh-asetukset myöhempää kirjautumista varten omalla uudella käyttäjällä komennoilla "sudo cp -rvn /root/.ssh/ /home/liljat/" ja "sudo chown -R liljat:liljat /home/liljat/"

Palasin komennolla "exit" paikalliselle virtuaalikoneelleni ja kokeilin kirjautua palvelimelle omalla uudella käyttäjälläni komennolla "ssh liljat@178.62.233.5". Kirjautuminen onnistui.

![kuva](https://github.com/user-attachments/assets/b7235b2d-ab84-4b1c-8891-d8ef4facbae3)

### Root-tunnuksen sulkeminen

Suljin root-tunnuksen komennoilla "sudo usermod --lock root" ja "sudo mv -nv /root/.ssh /root/DISABLED-ssh/". Poistin root-tunnuksen ssh-kirjautumisen käytöstä komennolla "sudoedit /etc/ssh/sshd_config", ja muokkasin tekstieditorissa kohdasta tekstin "PermitRootLogin yes" tekstiksi "PermitRootLogin no".

![kuva](https://github.com/user-attachments/assets/5d85602f-ce7f-4f3b-b80a-0920a9d0c162)

![kuva](https://github.com/user-attachments/assets/128e8d9e-8829-4c9e-b65d-1d977544e9ec)

Lopuksi annoin komennon "sudo service ssh restart", jotta uudet määritykset tulevat käyttöön.

### Päivitykset

Alustin päivitykset jälleen komennolla "sudo apt-get update", ja asensin saatavilla olevat päivitykset komennoilla "sudo apt-get upgrade" ja "sudo apt-get dist-upgrade".

### Lähteet

Karvinen 2017, First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS: 
https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/

Lehto 2022, Teoriasta käytäntöön pilvipalvelimen avulla (h4): https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/


## c) Weppipalvelimen asennus virtuaalipalvelimelle, 8.2.2024 klo 13.40

### Asentaminen

Asensin Apache2-weppipalvelimen omalle virtuaalipalvelimelleni komennolla "sudo apt-get install apache2" ja tarkistin asennuksen jälkeen palvelimen tilan komennolla "sudo systemctl status apache2". Palvelin oli aktiivisena.

![kuva](https://github.com/user-attachments/assets/229f4617-72f0-4bf1-a86a-387563100d1f)

Tein toisen reiän palomuuriin komennolla "sudo ufw allow 80/tcp". 

### Testaus

Testasin palvelimen toimivuutta kirjoittamalla virtuaalikoneeni nettiselaimeen palvelimen IP-osoitteen, ja testisivu aukesi onnistuneesti.

![kuva](https://github.com/user-attachments/assets/42101125-b64c-4e13-a0a8-20855a8ccdc0)

### Testisivun korvaaminen

Korvasin testisivun komennolla echo "Hei ja tervetuloa!"|sudo tee /var/www/html/index.html. Muutos näkyi sekä virtuaalikoneen selaimella että oman puhelimeni selaimella, eli sivu näkyy julkisesti.

![kuva](https://github.com/user-attachments/assets/f9597d55-299d-4b5e-9da9-e96be0bc5399)

### Lähteet

Tehtävänanto h4

Karvinen 2017, First Steps on a New Virtual Private Server – an Example on DigitalOcean and Ubuntu 16.04 LTS: 
https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/


