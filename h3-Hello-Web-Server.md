# h3 Hello Web Server

## x) Tiivistelmät

### Name-based Virtual Host Support
- Useampi host voi jakaa saman IP-osoitteen, kun IP-osoitteen sijaan käytetään verkkotunnusta määrittämään mitä palvelua palvelin tarjoaa
- Web-palvelin (kuten Apache) konfiguroidaan tunnistamaan eri host-nimet
- Kun palvelin saa pyynnön, se löytää parhaiten sopivan VirtualHost-argumentin pyynnön IP-osoitteen ja porttinumeron perusteella. Jos sama yhdistelmä on käytössä useammalla hostilla, palvelin vertailee ServerName- ja ServerAlias -direktiivejä. Jos täsmääviä tietoja ei löydy, käytetään listan ensimmäistä täsmäävää virtual hostia.
- Käytännössä jokaiselle hostille tulee luoda erillinen VirtualHost-blokki, joka sisältää vähintään ServerName-direktiivin (mitä hostia palvellaan) ja DocumentRoot-direktiivin (näyttää missä hostin sisältö sijaitsee tiedostojärjestelmässä)
- On järkevää luoda default virtual host, kun virtual host lisätään olemassa olevalle palvelimelle, sillä se helpottaa myöhempia määrityksiä ja toimia

Lähde: The Apache Software Foundation 2023, Name-based Virtual Host Support: https://httpd.apache.org/docs/2.4/vhosts/name-based.html


### Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address
- Apache mahdollistaa useampien verkkotunnusten toiminnan yhden IP-osoitteen alla
- Uuden hostin lisäys: sudoedit /etc/apache2/sites-available/x.example.com.conf, sudo a2ensite x.example.com, sudo systemctl restart apache2
- Nettisivun luominen normaalina käyttäjänä: mkdir -p /home/user/publicsites/x.example.com/, echo x > /home/user/publicsites/x.example.com/index.html
- Testaus: curl

Lähde: Karvinen 2018, Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/


## Alustus

Seuraavat harjoitukset on tehty virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

Kaikki harjoitukset perustuvat tehtävänantoihin kevään 2025 Linux-palvelimet -kurssin sivustolla: https://terokarvinen.com/linux-palvelimet/.


## a) Weppipalvelimen testaus, 29.1.2025 klo 13.30

Asensin Apache2-weppipalvelimen jo oppitunnin aikana. Testasin nettiselaimella palvelimen toimintaa kirjoittamalla hakukenttään "localhost". Tuntitehtävän aikana vaihdoimme oletusnettisivun käyttämään omaa sivua, joten localhost-haulla tämä näkymä aukesi.

![kuva](https://github.com/user-attachments/assets/0a962deb-e858-4530-9c27-86bc85b67b48)


## b) Lokit, 29.1.2025 klo 13.45

### Ongelmatilanne

Yritin ensin katsoa lokien muodostumista komennoilla "sudo tail /var/log/apache2/access.log" ja "tail -f /var/log/apache2/access.log", mutta ne näyttivät vain eilisen päivän tapahtumia. Yritin ladata localhost-sivua uudelleen sekä shift+refresh-painikkeiden yhdistelmällä, että sulkemalla koko selaimen ja avaamalla sitten localhostin uudelleen. Tämä ei auttanut.

Katsoin komennolla "sudo tail /var/log/apache2/error.log", muodostuuko sinne uusia lokeja. Täällä näkyi ajantasaiset lokimerkinnät. Tämän päivän ensimmäinen merkintä vaikuttaisi tekstin perusteella olevan ilmoitus siitä, että kaikki toimii normaalisti: "configured, resuming normal operations". 

![kuva](https://github.com/user-attachments/assets/9ded5908-2efb-47cc-8323-4ec67805a88d)


Tarkistin komennolla "sudo systemctl status apache2", mikä on weppipalvelimen tila, ja se oli running. Testasin kuitenkin, jos uudelleenkäynnistys komennolla "sudo systemctl restart apache2" auttaisi ongelmaani, mutta tästä ei ollut hyötyä. Kokeilin myös päivittää index.html:n sisältöä, ja se kyllä päivittyi localhost-sivulle, mutta ei auttanut lokien muodostumisessa. 

Jonkin aikaa selasin netissä keskustelupalstoja ja muita lähteitä, mutta en löytänyt tarpeeksi täsmällistä vastausta, jonka olisin ymmärtänyt. Viimeinen ajatukseni oli, että ehkä oma testisivuni ei jostain syystä toimi tässä yhteydessä, ja sen sijaan default-sivua voisi kokeilla. Olin aikaisemmin ottanut sen pois käytöstä komennolla "sudo a2dissite 000-default.conf", joten nyt palautin sen käyttöön komennolla "sudo a2ensite 000-default.conf". Palvelin piti vielä uudelleenkäynnistää komennolla "sudo systemctl restart apache2", ja tämän jälkeen lokit alkoivat heti toimimaan. 

En ole täysin varma, miksi tämä auttoi, ja jälkikäteen hain vielä netistä tietoa tarkemmilla hakusanoilla. Yhdellä keskustelupalstalla (https://stackoverflow.com/questions/68364578/why-do-http-requests-to-host-localhost-not-appear-in-apache2-access-logs) kuvattiin samankaltainen ongelma, jonka tapauksessa ilmeisesti asetukset estivät lokitietojen tallentumisen oikeaan paikkaan. En täysin ymmärtänyt mitä selityksessä tarkoitettiin, mutta jokseenkin hahmotin, että oikeaan paikkaan ei kohdistunut oikeita määrityksiä.


### Toimivat lokit

Jatkaessani harjoitusta latasin localhost-sivun pari kertaa uudestaan (Ctrl + refresh-painike), ja sain komennolla "tail -f /var/log/apache2/access.log" lokit talteen.

![kuva](https://github.com/user-attachments/assets/a16afc0b-1c8a-47c4-aa65-806fa43ef1d1)


### Ensimmäisen rivin analyysi 

Ensimmäisen lokitiedon rivillä oli ensin IP-osoite 127.0.0.1, joka on laitteen oma loopback osoite IPv4-verkossa, eli laite on tehnyt pyynnön itselleen. Access-lokissa IP-osoite on clientin, eli sen joka ottaa yhteyttä palvelimeen. Tässä tapauksessa client on oma laite eli virtuaalikone, kuten edellä todettu. (GeeksforGeeks 2025, ITtrip.)

Kaksi seuraavaa kohtaa oli merkitty viivoilla. Toinen kohta ilmeisesti tavallisestikin on tyhjä, ja kolmanteen kohtaan tulee tietoa vain, jos HTTP autentikointia on käytetty. (linuxconfig.org.)

Seuraavaksi rivillä on päivämäärä ja aika, milloin pyyntö on tapahtunut.

Tämän jälkeen on kirjattu itse pyyntö, tässä tapauksessa "GET / HTTP/1.1". "GET" ilmaisee, mitä HTTP-metodia on käytetty, ja jälkimmäinen osio kertoo mitä HTTP-protokollaa on käytetty (linuxconfig.org). Tarkensin tätä vielä ChatGPT:ssä, ja sen mukaan tämä merkintä on tapa pyytää palvelimen kotisivua käyttämällä HTTP/1.1-protokollaa. En löytänyt suoraan samaa tietoa internetlähteistä, mutta tekemäni pyyntö kyllä kohdistui palvelimen kotisivulle, mikä vahvistaisi väitteen. 

Seuraava kohta on statuskoodi 200 289. Statuskoodi palautetaan palvelimelta clientille, ja 200-alkuinen koodi tarkoittaa onnistumista (linuxconfig.org). Lähteen mukaan statuskoodin jälkeinen kohta kertoo pyydetyn tiedoston koon, mutta omissa lokitiedoissani se oli tyhjä. En löytänyt tietoa mistä tämä voisi kertoa, enkä myöskään keksinyt itse mahdollista syytä. Mahdollisesti pyyntöön ei liity palautettavaa tiedostoa?

Lopuksi rivillä on viittaava linkki, jos sitä on mahdollista käyttää, sekä tietoa clientin nettiselaimesta ja käyttöjärjestelmästä. Viittaava linkki kertoo, miten käyttäjä navigoi sivustolle. Omalla ensimmäisellä rivilläni ei mielestäni ole viittaavaa linkkiä. Seuraavaksi riville oli kirjattu Mozilla, mikä on virtuaalikoneen selain. X11 selittyi nopealla verkkohaulla ikkunointijärjestelmäksi (Wikipedia). Käyttöjärjestelmästä on mainittu Linux ja 64-bittinen versio. Loput tiedot (rv, Gecko, Firerfox...) ovat selaimeen ja sen moottoriin liittyviä tarkennuksia (Wikipedia, mdn web docs).

#### Lähteet:

GeeksforGeeks 2025, What is local host? https://www.geeksforgeeks.org/what-is-local-host/

ITtrip, Mastering Access Log Analysis in Linux: A Comprehensive Guide: https://en.ittrip.xyz/linux/linux-access-logs

linuxconfig.org 2023, Linux Apache log analyzer: https://linuxconfig.org/linux-apache-log-analyzer

Wikipedia, X Window System: https://en.wikipedia.org/wiki/X_Window_System

Wikipedia, Gecko (software): https://en.wikipedia.org/wiki/Gecko_(software)

Mdn web docs, Firefox user agent string reference: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent/Firefox


### Toisen rivin analyysi

Alkuosa oli GET-sanaan asti sama, kuin yllä. Seuraavaksi pyynnössä luki favicon.ico, ja favicon tarkoittaa pientä kuvaketta verkkosivuilla, joka näkyy mm. välilehdellä sivun ollessa auki selaimessa (lähde: https://en.wikipedia.org/wiki/Favicon). Sitä on siis pyydetty localhost-sivulta. Seuraavaksi rivillä onkin tuttu 404-alkuinen koodi, mikä indikoi epäonnistumista. Kuvaketta ei siis ole löytynyt, koska sellaista ei ole asetettu. 

Tällä rivillä on edellisessä osiossa mainittu viittaava linkki, eli oman tulkintani mukaan tästä käy ilmi, että käyttäjäjänä olen navigoinut sivulle suoraan rivillä näkyvän osoitteen avulla. Loput rivin tiedot ovat samat, kuin ylemmällä rivillä.


## c) Uusi name based virtual host, 31.1.2025 klo 17.52

Aloitin uuden name based virtual hostin luomisen virtuaalikoneen terminaalissa käyttäen opettajan laatimaa ohjesivustoa apuna. Aluksi tarkistin varmuudeksi, olihan minulla hakemisto nimellä "sites-available" polussa /etc/apache2/, ja se löytyi. Tämän jälkeen aloitin luomisen antamalla komennon "sudoedit /etc/apache2/sites-available/hattu.example.com.conf", ja kirjoitin avautuneeseen tekstieditoriin tarvittavat tiedot. 

![kuva](https://github.com/user-attachments/assets/649ef418-4403-48d5-980a-1e4bf9546ca2)

Komennolla "sudo a2ensite hattu.example.com" kytkin virtual hostin päälle, ja komennolla "sudo systemctl restart apache2" käynnistin palvelimen uudelleen. 

Loin uuden hakemiston normaalina käyttäjänä komennolla "mkdir -p /home/liljat/publicsites/hattu.example.com/", ja tarkistin, että se näkyy publicsites-hakemistossa komennolla "ls". 

![kuva](https://github.com/user-attachments/assets/aa19256c-898b-4481-ac64-795304091c88)

Lisäsin hakemistoon index.html-tiedoston komennolla "echo hattu > /home/liljat/publicsites/hattu.example.com/index.html". Tarkistin komentoja "cd" ja "ls" hyödyntäen, että tiedosto löytyi oikeasta sijainnista, ja sen sisältö oli annettu "hattu".

![kuva](https://github.com/user-attachments/assets/dd3f57c3-d16e-493a-a594-323ba02451cc)

Kävin katsomassa, mitkä sivut ovat päällä hakemistossa sites-enabled. Otin komennolla "a2dissite 000-default.conf lilja.example.com.conf" muut sivut pois päältä. 

![kuva](https://github.com/user-attachments/assets/b79773c3-72e0-4a16-8e69-54da37c1a263)

Kokeilin toimivuutta hakemalla nettiselaimella "localhost", ja uusi asetettu sivu aukesi onnistuneesti. Sana "hattu" löytyi tehtävänannon mukaisesti asetustiedoston nimestä, ServerName-muuttujasta ja etusivun sisällöstä.


## e) Validi HTML5-sivu, 31.1.2025 klo 18.23

Kopioin lyhyen HTML-sivun rakenteen opettajan artikkelista, ja muokkasin tekstisisältöä hattu-sivulle sopivaksi. Testasin sisällön HTML-validaattorilla ja sain kaksi huomautusta: kieliattributti suositeltiin lisäämään, ja ilmoitusluontoisesti kerrottiin, ettei rivillä 5 oleva kenoviiva vaikuta mihinkään. 

![kuva](https://github.com/user-attachments/assets/5921b8d1-080a-46b5-8fbd-59abfcf7e738)

Lisäsin kieliattribuutin w3.org-sivuston ohjeen perusteella toiselle riville muodossa "html lang="fi"", ja poistin rivin 5 kenoviivan. Muutosten jälkeen validaattori ei antanut uusia virheilmoituksia tai varoituksia. Alla kuvakaappaukset lopullisesta koodista ja sivustosta.

![kuva](https://github.com/user-attachments/assets/cda32952-12e8-4754-acdd-f1ffaf869c8a)

![kuva](https://github.com/user-attachments/assets/eb1584e4-86d7-4f2a-bcce-7d6d1c833296)


#### Lähteet:
Karvinen 2012, Short HTML5 page: https://terokarvinen.com/2012/short-html5-page/

W3C, Nu Html Checker: http://validator.w3.org/

W3C 2021, Declaring language in HTML: https://www.w3.org/International/questions/qa-html-language-declarations


## f) Curl, 1.2.2025 klo 16.30

Curl-komennolla voidaan siirtää tietoa eri protokollilla (esim. HTTPS). Komento yhdistettynä url-osoitteeseen palauttaa sivuston HTML-sisällön, esimerkiksi omalla virtuaalikoneellani "curl localhost" näyttää edellisessä tehtävässä laaditun HTML-sisällön. (GeeksforGeeks 2024.)

![kuva](https://github.com/user-attachments/assets/36ad6b10-b66a-4e52-a10f-86e195cec5a6)

Etsin tietoa "curl -I" -komennosta antamalla komennon "man curl". Manuaalista selvisi, että komento tuo vain sivuston otsikkotiedot.

![kuva](https://github.com/user-attachments/assets/4df101e2-2311-4505-bd9e-aa212fe97475)

Komento palautti ensin rivin, jossa HTTP-protokolla kuvataan onnistuneella 200-alkuisella koodilla ja OK-ilmaisulla. Seuraavilla riveillä oli GMT-päivämäärä, käytetty serveri ja viimeisin muokkausajankohta. ETag eli entity tag on osa HTTP-protokollaa, ja mahdollistaa mm. verkon välimuistin validoinnin ja tehostaa välimuistin käyttöä. Jos verkkopalvelimen sisältö ei ole muuttunut, sen ei tarvitse lähettää täydellistä vastausta. (Wikipedia.)

Accept-Ranges on otsikko, joka kertoo, että palvelin sallii myös osittaiset pyynnöt lähteestä, ja "bytes" kertoo yksikön, jolla määritellään tämä alue. Content-Length ilmaisee lähetetyn viestin koon tavuissa. Vary kertoo, mitkä osat pyynnöstä vaikuttivat vastauksen sisältöön. Accept-Encoding kertoo, minkä enkoodauksen lähettäjä voi ymmärtää. Content-Type ilmaisee alkuperäisen mediatyypin ennen sisällön enkoodausta. (mdn web docs.)


#### Lähteet:
GeeksforGeeks 2024, curl Command in Linux with Examples: https://www.geeksforgeeks.org/curl-command-in-linux-with-examples/

Wikipedia 2024, HTTP ETag: https://en.wikipedia.org/wiki/HTTP_ETag

mdn web docs, Accept-Ranges: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Ranges

mdn web docs, Content-Length: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Length

mdn web docs, Vary: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Vary

mdn web docs, Accept-Encoding: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Encoding

mdn web docs, Content-Type: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type




