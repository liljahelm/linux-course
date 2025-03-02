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

Aloitin tehtävän 1.3.2025 klo 13. Tarkoituksena oli testata oman sivun TLS laadunvarmistustyökalulla. Tehtävänannossa mainittiin SSL Labs, joten päätin hyödyntää sitä. Syötin hakukenttään verkkotunnukseni https-muodossa. Kokonaistulos oli A eli testaus onnistui yleisellä tasolla hyvin, mutta muutamia puutteitakin ilmeni. Kirjasin nämä ylös ja palasin raportin kirjoittamiseen 2.3.2025 klo 14.15.

![kuva](https://github.com/user-attachments/assets/64623340-00d4-44a5-be33-be37f63d53dc)

### DNS CAA

Ensimmäinen huomautus oli, ettei DNS CAA:ta ollut. CAA on DNS-tietue, joka sallii sivuston omistajan määritellä, mitkä CA:t eli sertifikaattien myöntäjät ovat sallittu myöntämään omalle domainille sertifikaatin. Oletuksena jokainen julkinen CA saa myöntää sertifikaatteja kaikille domainnimille julkisessa DNS:ssä, kunhan ne osoittavat hallinnoivansa kyseistä domainia. (Let's Encrypt 2023.) CAA-tiedot pitäisi lisätä domainille erikseen, jos haluaisin rajoittaa hyväksyttyjä CA:ita. CAA:n käyttö vähentäisi kaiken kaikkiaan hyväksyttyjen sertifikaattien myöntäjien määrää, mikä vähentäisi myös potentiaalisten väärinkäyttäjien määrää. En kuitenkaan lähtenyt vielä lisäämään sivustolleni CAA:ta, mutta mikäli jatkan sivuston kehittämistä ja ylläpitämistä tämän kurssin jälkeen, ajattelen että CAA:n lisääminen olisi pieni hyödyllinen teko tietoturvan parantamiseksi.

![kuva](https://github.com/user-attachments/assets/fc5b243a-c7e4-4b72-9823-4b15e825843b)

### Cipher Suites

Kohdassa Cipher Suites muutama kohta oli merkitty heikoksi. Cipher suite tarkoittaa algoritmien sarjaa, jolla verkkoyhteys suojataan, ja sarjat yleensä käyttävät tässä TLS:ää (Wikipedia 2024). Heikkoudet ilmenivät testissä TLS 1.2:n alla, joka on nykyistä TLS 1.3:a edeltävä versio protokollasta. TLS 1.2 on ollut käytössä vuodesta 2008 ja TLS 1.3 vuodesta 2018 asti. (Wikipedia 2025.) Vanhemman version jotkin salausmekanismit ovat siis vanhentuneita, eivätkä riittävän tehokkaita. Testituloksen perusteella yhteyden salaamisessa on kuitenkin käytössä molemmat versiot.

![kuva](https://github.com/user-attachments/assets/9c863a79-bdf2-4977-beac-36b6a1310df8)

Poistamalla TLS 1.2 ilmoitetut heikkoudet ehkä saataisiin pois, joten päätin kokeilla lisätä konfiguraatiotiedostoon rivin ````SSLProtocol -all +TLSv1.3```` (Rahul 2024), joka poistaisi vanhat versiot käytöstä ja ottaisi käyttöön vain TLS 1.3:n. Tein testin uudestaan SSL Labsin sivulla ja tulokset muuttuivat.

![kuva](https://github.com/user-attachments/assets/674d5a9e-5759-4567-bff9-30b48ea8bb5a)

Nyt samoja heikkousilmoituksia ei ollut, mutta TLS 1.2 otsikko oli silti samalla keltaisella värillä korostettu. Myös kättelyissä ilmeni uusia virheilmoituksia liittyen protokollaversioon tietyillä selaimilla, ei yhteydenmuodostus ei onnistu joillakin vanhoilla selaimilla. Muokkasin konfiguraatiotiedoston SSLProtocol riviä seuraavanlaiseksi: ````SSLProtocol -all +TLSv1.2 +TLSv1.3````, ja kokeilin onko tällä vaikutusta TLS 1.2:n otsikon väriin. Käynnistin palvelimen uudestaan, tyhjensin selaimen välimuistin ja tein testin uudelleen, mutta tulos ei muuttunut.

Päätin ottaa TLS 1.2:n lopulta pois määrityksistä ja jättää vain TLS 1.3:n, sillä ymmärrykseni mukaan TLS 1.3 tarjoaa parhaat tietoturvaominaisuudet ja on nopeampi (Wikipedia 2024). TLS 1.2:n voisi mielestäni sallia, jos sivuston käyttäjillä olisi ehdoton tarve asioida sivustolla, mutta heidän käyttöjärjestelmänsä eivät tukisi 1.3:sta eikä niitä olisi mahdollista päivittää uusimpaan versioon. 

### Handshake Simulation

Kättelyissä ilmeni ensin vain yksi virheilmoitus Chrome 49:n kohdalla:

![kuva](https://github.com/user-attachments/assets/529cabea-4c77-418b-bbe9-ce1a0db8ee7e)

Kun muokkasin käytettyä TLS-versiota, ilmoituksia tuli kolme uutta: 

![kuva](https://github.com/user-attachments/assets/1bc07636-7add-4023-9212-4a6b711429ab)

Vanhoilla selaimilla turvallisen yhteyden muodostus ei siis onnistu sivustolleni, eikä myöskään vanha OpenSSL-versio ole tuettu. OpenSSL on avoimen lähdekoodin ohjelmistokirjasto, jota sovellukset käyttävät salattujen yhteyksien luomisessa. Virheilmoituksen version 1.1.0 tuki on loppunut vuonna 2019, ja nykyään käytössä on numerolla 3 alkavat versiot, uusimpana 3.4.0. (Wikipedia 2025.) Mielestäni virheilmoituksiin voi siis jättää reagoimatta, sillä versiot ovat niin vanhoja, ettei niillä ole varmastikaan suurta käyttöä, enkä näin ollen koe että näillä pitäisi olla mahdollista muodostaa salattu yhteys sivustolleni.

Testissä oli myös luettelo lukuisia eri clienteja joilla kättelysimulointia ei tehty, syynä "Protocol mismatch" eli ei-yhteensopiva protokolla. Tämä luultavasti liittyy ylempään vaiheeseen, jossa sallin vain TLS 1.3:n käytön. Kaikki listalla olleet kohteet olivat jälleen vanhoja versioita, joten mielestäni ei haittaa, vaikka simulointia ei voitu tehdä.

![kuva](https://github.com/user-attachments/assets/7af9d202-885b-4606-a69e-3fd7b3cc0436)
![kuva](https://github.com/user-attachments/assets/1f2101af-bd4d-4fd2-88fc-6d56d36a665e)
![kuva](https://github.com/user-attachments/assets/0a05ddaa-6ed8-408d-a738-93c23d4b0b2c)

Tehtävään ja raportointiin kului aikaa 1h 30min.

### Lähteet

Karvinen 2025, Linux Palvelimet 2025 alkukevät, Läksyt, h6 Salataampa: https://terokarvinen.com/linux-palvelimet/#h6-salataampa. Luettu 2.3.2025.

Qualys, SSL Labs, SSL Server Test: https://www.ssllabs.com/ssltest/index.html. Käytetty 1.3. ja 2.3.2025.

Let's Encrypt 2023, Certificate Authority Authorization (CAA): https://letsencrypt.org/docs/caa/. Luettu 2.3.2025.

Wikipedia 2024, Cipher suite: https://en.wikipedia.org/wiki/Cipher_suite. Luettu 2.3.2025.

Rahul 2024, Techadmin.net, How to enable TLS 1.3, TLS1.2 only in Apache: https://tecadmin.net/enable-tls-version-apache/. Luettu 2.3.2024.

Wikipedia 2025, OpenSSL: https://en.wikipedia.org/wiki/OpenSSL. Luettu 2.3.2025.
 

## c) Vapaaehtoinen: Weppilomake

Tarkoituksena oli tehdä weppilomake, jossa on käyttäjätunnus ja salasana, ja käyttää salaamatonta http-yhteyttä. Liikennettä tuli siepata ja siitä tuli tehdä havaintoja.

Aloitin HTML-lomakkeen luomisen 2.3.2025 klo 15.40. Käytin apuna Geeks for geeks -sivuston ohjetta (Geeks for geeks 2024). Muokkasin HTML-koodin suoraan palvelimella index.html-tekstitiedostoon.

![kuva](https://github.com/user-attachments/assets/759b7ded-8060-49db-9889-101c31d3b096)

Lomake ilmestyi heti tallentamisen jälkeen näkyviin sivustolleni.

![kuva](https://github.com/user-attachments/assets/d5e4b75c-3113-4628-8bf7-6fee1cdb99d1)

### Wireshark

Sieppasin liikennettä ensin Wiresharkilla, joka oli jo asennettuna tietokoneelleni. Valitsin interfaceksi Ethernetin, koska arvelin selaimeni verkkoliikenteen tapahtuvan siellä. Aloitin tallennuksen, siirryin sivustolleni http-yhteydellä, syötin lomakkeelle käyttäjätunnuksen "käyttis" ja salasanan "salasana", ja valitsin "kirjaudu sisään". Lopetin tallennuksen ja lähdin perkaamaan tallennettuja paketteja. Nopeasti silmiini osui HTTP-paketti, jossa luki selkeästi (ä-kirjainta lukuunottamatta) syöttämäni tiedot. 

![kuva](https://github.com/user-attachments/assets/66a5138a-d6ef-43a6-8126-ec6d216912be)

### Ngrep

Kokeilin myös ngrepiä. Hain saatavilla olevat päivitykset komennolla ````sudo apt-get update```` ja asensin ngrepin ````sudo apt-get install ngrep````. Löysin ensin netistä komennon ````sudo ngrep -q '^GET .* HTTP/1.1```` (Geeks for geeks 2021), jolla piti pystyä sieppaamaan paketteja nettiselaimelta, mutta se ei tuottanut mitään vastausta. Kokeilin aikaa säästääkseni Copilotin tekoälyä, ja kysyin miksei komento syötä mitään vastausta. Sain vastaukseksi tarkistetun komennon ````sudo ngrep -d any '^GET .* HTTP/1.1'````. Copilotin mukaan ````-d any```` tarkoittaa, että ngrep kuuntelee kaikkia saatavilla olevia verkkoliitäntöjä, ja kohdan ````any```` olisi voinut korvata oikealla verkkoliitännällä. En kuitenkaan tiennyt oikeaa liitäntää, joten jätin komennon tuollaseksi.

Syötin komennon ja kävin antamassa lomakkeella taas käyttäjätunnuksen ja salasanan. Tiedot päivittyivät terminaaliin heti, ja jälleen molemmat syötetyt tiedot näkyivät selvällä tekstillä tulosteessa. Näppäinyhdistelmä ````Ctrl + c```` lopetti tallentamisen terminaalissa.

![kuva](https://github.com/user-attachments/assets/8e67896b-9032-4314-8240-6fd54345f5a6)

### Tietoturvasta

Havaintojeni perusteella salaamaton yhteys (http) lähettää lomakkeelle annetut tiedot sellaisenaan selvässä tekstimuodossa ilman mitään salausta. Tietojen näkyminen selkeänä tekstinä mahdollistaa niiden kaappaamisen ja käyttämisen esimerkiksi identiteettivarkauteen. Tämän vuoksi on tärkeää huolehtia sivuston yhteyksien salaamisesta, jos sivustolla on tarkoitus käsitellä, lähettää ja/tai vastaanottaa käyttäjien tietoja. Http-yhteys ei ole ollenkaan tietoturvallinen vaihtoehto tähän, vaan sen sijaan tulee käyttää https:ää, johon tämänkin harjoituksen aikana on tutustuttu. Sertifikaatin hankkiminen luotetulta myöntäjältä oli helppoa ja ilmaista, joten hyviä perusteluja https:n käyttämättä jättämiseen ei ihan heti tule mieleen. 

Tähän tehtävävaiheeseen raportointeineen kului aikaa 1h.

### Lähteet

Geeks for geeks 2024, HTML Login Form: https://www.geeksforgeeks.org/html-login-form/. Luettu 2.3.2025.

Geeks for geeks 2021, Ngrep – Network Packet Analyzer for Linux: https://www.geeksforgeeks.org/ngrep-network-packet-analyzer-for-linux/. Luettu 2.3.2025.

