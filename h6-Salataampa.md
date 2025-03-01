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

### Lähteet


## b) TLS:n testaus

### Lähteet


## c) Vapaaehtoinen: Weppilomake
