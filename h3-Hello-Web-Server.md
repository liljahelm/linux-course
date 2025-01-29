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
- Oletusnettisivun korvaaminen: echo "Default"|sudo tee /var/www/html/index.html
- Uuden hostin lisäys: sudoedit /etc/apache2/sites-available/pyora.example.com.conf, cat /etc/apache2/sites-available/pyora.example.com.conf, sudo a2ensite pyora.example.com, sudo systemctl restart apache2
- Nettisivun luominen normaalina käyttäjänä: mkdir -p /home/xubuntu/publicsites/pyora.example.com/, echo pyora > /home/xubuntu/publicsites/pyora.example.com/index.html
- Testaus: curl

Lähde: Karvinen 2018, Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/


## Alustus

Seuraavat harjoitukset on tehty virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

Kaikki harjoitukset perustuvat tehtävänantoihin kurssin sivustolla: https://terokarvinen.com/linux-palvelimet/.


## a) Weppipalvelimen testaus, 29.1.2025 klo 13.30

Asensin Apache2-weppipalvelimen jo oppitunnin aikana. Testasin nettiselaimella palvelimen toimintaa kirjoittamalla hakukenttään "localhost". Tuntitehtävän aikana vaihdoimme oletusnettisivun käyttämään omaa sivua, joten localhost-haulla tämä näkymä aukesi.

![kuva](https://github.com/user-attachments/assets/0a962deb-e858-4530-9c27-86bc85b67b48)


## b) Lokit 29.1.2025 klo 13.45

Yritin ensin katsoa lokien muodostumista komennoilla "sudo tail /var/log/apache2/access.log" ja "tail -f /var/log/apache2/access.log", mutta ne näyttivät vain eilisen päivän tapahtumia. Yritin ladata localhost-sivua uudelleen sekä shift+refresh-painikkeiden yhdistelmällä, sekä sulkemalla koko selaimen ja avaamalla sitten localhostin uudelleen. Tämä ei auttanut.

Katsoin komennolla "sudo tail /var/log/apache2/error.log", muodostuuko sinne uusia lokeja. Täällä näkyi ajantasaiset lokimerkinnät. Tämän päivän ensimmäinen merkintä vaikuttaisi tekstin perusteella olevan ilmoitus siitä, että kaikki toimii normaalisti: "configured, resuming normal operations". 

![kuva](https://github.com/user-attachments/assets/9ded5908-2efb-47cc-8323-4ec67805a88d)



Tarkistin komennolla "sudo systemctl status apache2", mikä on weppipalvelimen tila, ja se oli running. Testasin kuitenkin, jos uudelleenkäynnistys komennolla "sudo systemctl restart apache2" auttaisi ongelmaani, mutta tästä ei ollut hyötyä. Kokeilin myös päivittää index.html:n sisältöä, ja se kyllä päivittyi localhost-sivulle, mutta ei auttanut lokien muodostumisessa. 

Jonkin aikaa selasin netissä keskustelupalstoja ja muita lähteitä, ja yritin löytää ratkaisua. Mitään hyödyllistä ei löytynyt, enkä luultavasti osannut hakea tarpeeksi osuvilla hakusanoilla. Viimeinen ajatukseni oli, että ehkä oma testisivuni ei jostain syystä toimi tässä yhteydessä, ja sen sijaan default-sivua voisi kokeilla. Olin aikaisemmin ottanut sen pois käytöstä komennolla "sudo a2dissite 000-default.conf", joten nyt palautin sen käyttöön komennolla "sudo a2ensite 000-default.conf". Palvelin piti vielä uudelleenkäynnistää komennolla "sudo systemctl restart apache2", ja tämän jälkeen lokit alkoivat heti toimimaan. 

En ole täysin varma, miksi tämä auttoi, ja jälkikäteen hain vielä netistä tietoa tarkemmilla hakusanoilla. Yhdellä keskustelupalstalla (https://stackoverflow.com/questions/68364578/why-do-http-requests-to-host-localhost-not-appear-in-apache2-access-logs) kuvattiin samankaltainen ongelma, jonka tapauksessa ilmeisesti asetukset estivät lokitietojen tallentumisen oikeaan paikkaan. Olen vielä niin aloittelija, etten ymmärtänyt selitystä kovinkaan syvällisesti, mutta pinnallisella tasolla se auttoi hieman hahmottamaan asiaa. 

Jatkaessani harjoitusta latasin localhost-sivun pari kertaa uudestaan, ja sain komennolla "tail -f /var/log/apache2/access.log" lokit talteen.

![kuva](https://github.com/user-attachments/assets/a16afc0b-1c8a-47c4-aa65-806fa43ef1d1)






## c) Uusi name based virtual host
## e) Validi HTML5-sivu
## f) Curl
