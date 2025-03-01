# h6 Salataampa

## Alustus

Seuraavat harjoitukset on tehty virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

Virtuaalipalvelin on vuokrattu DigitalOcean-palvelusta, ja palvelimella on 1 Gt keskusmuistia ja 25 Gt:n SSD-levy.

Kaikki harjoitukset perustuvat tehtävänantoihin kevään 2025 Linux-palvelimet -kurssin sivustolla: https://terokarvinen.com/linux-palvelimet/.


## x) Tiivistelmät

### How It Works

- Let's Encrypt ja ACME-protokolla mahdollistaa sertifikaatin hankkimisen automaattisesti ajamalla varmenteiden hallinta-agentin (certificate management agent) web-palvelimella
- Agentti saa CA:lta (Let's Encrypt) avainparin (authorized key pair), kun agentti on suorittanut CA:lta saadut haasteet ja todentanut hallinnoivansa domainia, jolle sertifikaattia haetaan
- Haasteet voivat pyytää mm. DNS-tietueen luomista domainin alle tai tarjoamalla HTTP-resussi tunnetun URI:n (Uniform Resource Identifier) alla domainissa. Haasteiden lisäksi agentin pitää allekirjoittaa yksityisellä avaimellaan todistaakseen, että se hallitsee avainparia.
- Avainparilla allekirjoitetaan kaikki hallinnointipyynnöt (management request), joilla voidaan pyytää, uusia tai poistaa käytöstä sertifikaatti
- Jos hallinnointipyyntö hyväksytään, se antaa sertifikaatin domainille pyynnön julkisella avaimen kanssa, sekä antaa sen takaisin agentille ja myös lukuisiin julkisiin Certificate Transparency (CT) lokeihin. Sertifikaatin poistaminen toimii samalla tavalla, ja luotettuja osapuolia inofrmoidaan olemaan hyväksymättä käytöstä poistettua sertifikaattia.

Lähde: Let's Encrypt 2024, How It Works: https://letsencrypt.org/how-it-works/. Luettu 1.3.2025.


### Obtain a Certificate: Using an existing, running web server

- Olemassa olevalle web-palvelimelle, joka toimii portissa 80 http-protokollalla, vaaditaan myös ````--http.webroot````-vaihtoehto, joka kirjoittaa ````http-01````-haasteen tunnuksen annettuun kansioon hakemistossa, eikä käynnistä palvelinta
- Annetun hakemiston pitää olla julkisesti saatavilla ja toimia domainin juurihakemistona, jotta validointi onnistuu
- Jos hakemisto ei ole julkisesti saatavilla, pitää uudelleenkirjoittaa pyynnöt hakemistoon oikeaan polkuun

Lähde: Lange 2024, Lego, Obtain a Certificate: Using an existing, running web server: https://go-acme.github.io/lego/usage/cli/obtain-a-certificate/index.html#using-an-existing-running-web-server. Luettu 1.3.2025.

### SSL/TLS Strong Encryption: How-To, Basic Configuration Example'

SSL-konfigruaatiotiedoston pitää sisältää vähintään seuraavat tiedot:
- LoadModule ssl_module modules/mod_ssl.so
- Portti 443
- SSLEngine on
- SSLCertificateFile "/path/to/www.example.com.cert"
- SSLCertificateKeyFile "/path/to/www.example.com.key"


Lähde: The Apache Software Foundation 2025, SSL/TLS Strong Encryption: How-To: Basic Configuration Example: https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html#configexample. Luettu 1.3.2025.


## a) TLS-sertifikaatin hankkiminen ja asentaminen

Aloitin tehtävän tekemisen 1.3.2025 klo 11.35. Tarkoituksena on hankkia omalle virtaalipalvelimelleni ilmainen TLS-sertifikaatti Let's Encryptilta, ja osoittaa sen toimivuus. 

Aluksi kirjauduin SSH:lla virtuaalipalvelimelle ja käynnistin palvelimen uudelleen komennolla ````sudo systemctl restart apache2````, ja testasin nettisivun toimivuutta varmistaakseni toimivan alkutilanteen. Kumpikin toimi ongelmitta. Asensin legon komennolla ````sudo apt-get install lego````. Lego on Let's Encryptin asiakasohjelma ja kirjasto, jolla hankitaan TLS-sertifikaatti domainille (Fernandez 2025). 

Tiesin, ettei domainillani ole vielä sertifikaattia, mutta testasin kuitenkin hakea sivuani osoitteesta ````https://liljatatti.me````, eikä sivusto luonnollisesti auennut sitä kautta.

![kuva](https://github.com/user-attachments/assets/d3f2811b-e5b8-42f4-90a3-5722ebc6f79a)


Sertifikaattia piti ensin hakea testipalvelimella (staging), mikä annettiin tulevaan komentoon ````server````-parametrin arvoksi.
Kopioin oikean komennon tehtävänannosta ja muutin omat tietoni valmiiden tietojen tilalle:

````lego --server=https://acme-staging-v02.api.letsencrypt.org/directory --accept-tos --email="lilja.tatti@hotmail.com" --domains="liljatatti.me" --domains="www.liljatatti.me" --http --http.webroot="/home/liljat/publicsites/liljatatti.me/" --path="/home/liljat/lego/certificates/" --pem run````

Haku onnistui.

![kuva](https://github.com/user-attachments/assets/d660f16d-5dbb-4bf3-bd01-15f762849e45)


Seuraavaksi piti poistaa ````--path````-osoittama hakemisto, johon oli muodostunut testitietoa. Käytin komentoa ````rm -r /home/liljat/lego/certificates/````. ````-r```` poistaa myös kaiken sisällön.

Aiemmin annetusta komennosta poistettiin ````--server````-parametri, jotta pyyntö menisi oikealle palvelimelle, joten käytetty komento näytti seuraavalta:

````lego --accept-tos --email="lilja.tatti@hotmail.com" --domains="liljatatti.me" --domains="www.liljatatti.me" --http --http.webroot="/home/liljat/publicsites/liljatatti.me/" --path="/home/liljat/lego/certificates/" --pem run````

Haku onnistui.

![kuva](https://github.com/user-attachments/assets/80eb74ee-a8b3-4b1d-be05-bcd2523eb38a)


Olin hakemistopolussa ````/etc/apache2/sites-available```` ja lisäsin konfigurointitiedostoon seuraavat direktiivit komennolla ````sudoedit liljatatti.me.conf````:

![kuva](https://github.com/user-attachments/assets/844d82b9-7675-45c4-9101-b48245a7aa54)



Otin SSL:n käyttöön komennolla ````sudo a2enmod ssl````, ja testasin antamani konfigurointitiedot komennolla ````sudo apache2ctl configtest````. Testaus epäonnistui syntaksivirheen vuoksi.

![kuva](https://github.com/user-attachments/assets/06d386da-d9e6-4242-aab8-6c3e2a33b8c5)


Olin laittanut väärät polut SSL-tietojen kohdalle, eli ylimääräisenä sanana polussa oli ````publicsites````, missä lego-hakemisto ei oikeasti sijaitse. Korjasin polut komennolla ````sudoedit liljatatti.me.conf````.

![kuva](https://github.com/user-attachments/assets/f9733e3a-cbdb-4454-a79b-f1a5bd481494)


Kokeilin testiä uudelleen, mutta edelleen ilmestyi virheilmoitus syntaksivirheestä.

![kuva](https://github.com/user-attachments/assets/8ac6d7b9-8ed7-4497-9706-fb7cb34b14cb)


Kävin tarkistamassa hakemistopolkua menemällä hakemistoon ````/home/lego/liljat/lego/````. Siellä oli kansio certificates, mutta vasta sen alla oli oikeat kansiot ````accounts```` ja ````certificates````. Polussa ````/home/lego/liljat/lego/certificates/certificates```` oli tarvittavat tiedostot. Korjasin siis jälleen konfiguraatiotiedoston polkuja lisäämällä toisen ````certificates````.

![kuva](https://github.com/user-attachments/assets/7e37abcd-0a45-426f-886f-774d42106378)


Sain vielä yhden virheilmoituksen testikomennolla. 

![kuva](https://github.com/user-attachments/assets/efa9b5f0-5bea-431d-af8b-c0404aa4415f)


Syntaksi oli siis oikein, mutta palvelimen nimeä ei jostain syystä pystytty määrittelemään. Lisäsin kehotuksen mukaisesti konfiguraatiotiedoston alkuun globaalisti uuden ````ServerName```` maininnan.

![kuva](https://github.com/user-attachments/assets/ebcc5453-e66d-418f-bb5e-f16520cacb43)


Tämän jälkeen testi onnistui, ja käynnistin palvelimen uudelleen, jotta muutokset tulevat voimaan.

![kuva](https://github.com/user-attachments/assets/b4b2cf49-fc07-4cac-9409-5ee31bdf96f9)


Lopuksi avasin palomuuriin reiän portille 443, jotta yhteys voidaan muodostaa.

![kuva](https://github.com/user-attachments/assets/3bc058f1-c871-4b05-89a9-f1306d2845d8)


Testasin sivuston toimivuutta hakemalla ````https://liljatatti.me```` sekä virtuaalikoneen että oman tietokoneeni Firefox-selaimilla, ja molemmat avasivat sivun onnistuneesti.

![kuva](https://github.com/user-attachments/assets/a017cfc5-8524-491a-b40e-bf2b6fec1866)

![kuva](https://github.com/user-attachments/assets/105ffc89-beb8-4575-86b8-542c0febb055)


Aikaa tehtävän tekemiseen ja samanaikaiseen raportin kirjoittamiseen kului 1h 25min.


### Lähteet

Fernandez 2025, Lego: https://go-acme.github.io/lego/. Luettu 1.3.2025.

Karvinen 2025, Linux Palvelimet 2025 alkukevät, Läksyt, h6 Salataampa: https://terokarvinen.com/linux-palvelimet/#h6-salataampa. Luettu 1.3.2025.


## b) TLS:n testaus

Aloitin tehtävän 1.3.2025 klo 13. Tarkoituksena oli testata oman sivun TLS laadunvarmistustyökalulla. Tehtävänannossa mainittiin SSL Labs, joten päätin hyödyntää sitä. Syötin hakukenttään verkkotunnukseni https-muodossa. Kokonaistulos oli A eli testaus onnistui yleisellä tasolla hyvin, mutta muutamia puutteitakin ilmeni. 

![kuva](https://github.com/user-attachments/assets/64623340-00d4-44a5-be33-be37f63d53dc)

Ensimmäinen huomautus oli, ettei DNS CAA:ta ollut. CAA on DNS-tietue, joka sallii sivuston omistajan määritellä, mitkä CA:t eli sertifikaattien myöntäjät ovat sallittu myöntämään omalle domainille sertifikaatin. Oletuksena jokainen julkinen CA saa myöntää sertifikaatteja kaikille domainnimille julkisessa DNS:ssä, kunhan ne osoittavat hallinnoivansa kyseistä domainia. (Let's Encrypt 2023.) CAA-tiedot pitäisi lisätä domainille erikseen, jos haluaisin rajoittaa hyväksyttyjä CA:ita. CAA:n käyttö vähentäisi kaiken kaikkiaan hyväksyttyjen sertifikaattien myöntäjien määrää, mikä vähentäisi myös potentiaalisten väärinkäyttäjien määrää. En kuitenkaan lähtenyt vielä lisäämään sivustolleni CAA:ta, 

![kuva](https://github.com/user-attachments/assets/fc5b243a-c7e4-4b72-9823-4b15e825843b)

Kohdassa Cipher Suites muutama kohta oli merkitty heikoksi, 

![kuva](https://github.com/user-attachments/assets/9c863a79-bdf2-4977-beac-36b6a1310df8)

Kättelyissä Chrome 49 antoi ilmoituksen:

![kuva](https://github.com/user-attachments/assets/529cabea-4c77-418b-bbe9-ce1a0db8ee7e)



### Lähteet

Let's Encrypt 2023, Certificate Authority Authorization (CAA): https://letsencrypt.org/docs/caa/. Luettu 1.3.2025.

## c) Vapaaehtoinen: Weppilomake
