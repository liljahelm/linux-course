# h5 Nimekäs

## Alustus

Seuraavat harjoitukset on tehty virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

Kaikki harjoitukset perustuvat tehtävänantoihin kevään 2025 Linux-palvelimet -kurssin sivustolla: https://terokarvinen.com/linux-palvelimet/.

## a) Nimi, 15.2.2025 klo 14 

Hankin itselleni domainnimen NameCheapin kautta, sillä sitä suositeltiin oppitunnilla ja GitHub Educationin kautta sai palveluun vuodeksi ilmaisen nimen. Hyödynsin tarvittavissa kohdissa Susanna Lehdon tehtäväraporttia ohjeena.

Ensin hakeuduin NameCheapin sivustolle GitHubin Student Developer Packin linkin kautta, ja rekisteröidyin. Rekisteröityminen piti tehdä henkilökohtaisella sähköpostilla koulun sähköpostin sijaan. Alussa valitsin kohdan GitHub Pages Free siltä varalta, jos haluaisin myöhemmin hyödyntää nettisivun tekemistä GitHubin repositoryjen kautta. 

![kuva](https://github.com/user-attachments/assets/95f2716d-4671-412e-9b4f-5577b95b587e)

Tilaus onnistui ja pääsin jatkamaan kirjautumiseen ja tarpeellisten toimien tekemiseen. Kirjauduttuani palveluun valitsin valikosta ````Domain List````, josta pääsin hallintapaneeliin ja valitsin valikon ````Advanced DNS````. Näkymässä oli aluksi viisi (kuvassa neljä) valmista tietuetta, jotka poistin, sillä niistä ei ole tässä yhteydessä hyötyä. 

![kuva](https://github.com/user-attachments/assets/07c06c77-57a1-4416-987b-3a025580e6a7)

Lisäsin kaksi uutta A-tietuetta (A Record) oman virtuaalipalvelimeni IP-osoitteella. A-tietue on yksi olennaisimmista DNS-tietueista, ja se yhdistää IP-osoitteen annettuun domainnimeen (Cloudfare). Host-kohtiin valitsin ````@```` ja ````www````, jotta sivulle pääsee sekä suoraan muodossa ````liljatatti.me```` että www-muodossa ````www.liljatatti.me````. TTL eli Time To Live -kenttään valitsin 5 min, eli DNS-palvelin saa säilyttää tietuetta viisi minuuttia ennen, kuin sitä pitää pyytää uudelleen. Lyhyt TTL-aika mahdollistaa DNS-tietojen nopean päivittymisen, mutta lisäävät verkkoliikennettä. (Wikipedia.) Lopuksi tallensin tiedot.

![kuva](https://github.com/user-attachments/assets/d643b5cf-bddb-4739-b1fe-430637d5a15e)

Viimeiseksi kokeilin hakea oman tietokoneeni selaimella sivua ````liljatatti.me````, ja sieltä avautui oikeasta IP-osoitteesta aiemmin luotu sivusto, eli tehtävä onnistui tältä osin.

![kuva](https://github.com/user-attachments/assets/a8692f67-f97b-4f26-a7f0-b334d18a49e0)

Aikaa tähän kului 20 minuuttia.

### Lähteet

Lehto 2022, Teoriasta käytäntöön pilvipalvelimen avulla (h4): https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/. Luettu 15.2.2025.

Cloudfare, DNS A record: https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/. Luettu 15.2.2025.

Wikipedia 2019, Time to Live: https://fi.wikipedia.org/wiki/Time_to_Live. Luettu 15.2.2025.


## b) Name Based Virtual Host, 15.2.2025 klo 14.50

Aloitin luomaan uutta Name Based Virtual Hostia virtuaalipalvelimen terminaalissa, koska ajattelin, että palvelimelle tulevat sivut tulisi luoda siellä. Loin uuden tiedoston komennolla ````/etc/apache2/sites-available/liljatatti.me.conf````, ja tarkistin tekstieditoriin lisäämäni sisällön cat-komennolla. Tässä vaiheessa sisältö vaikutti mielestäni virheettömältä, sillä olin huolellisesti tehnyt sen ohjeiden (Karvinen 2018) mukaan ja kopioinut tiedostopolut ilman kirjotusvirheitä.

### Ongelmatilanne

Jatkoin Tero Karvisen sivuston ohjeen mukaisesti ja annoin komennon ````sudo a2ensite liljatatti.me````, ja sen jälkeen komennon ````sudo systemctl restart apache2```` käynnistääkseni palvelimen uudelleen. Sain kuitenkin seuraavan kuvan mukaisen virheilmoituksen.

![kuva](https://github.com/user-attachments/assets/cf6cb48f-9006-4edf-a276-38c55647a3fc)

Annoin kehotuksen mukaisen komennon ````systemctl status apache2.service````, ja sieltä selvisi, ettei Apache2-palvelin ole aktiivisena. 

![kuva](https://github.com/user-attachments/assets/0be55294-efa7-40f3-afc5-9806b4fc8038)

En saanut palvelinta käynnistettyä komennolla ````systemctl start apache2````, vaan lopputulos oli sama kuin restart-komennon jälkeen. 

Jatkoin yrittämistä ja tässä kohtaa aloitin vaiheet alusta ja kokeilin poistua palvelimen terminaalista, ja palata uudelleen. Tässä vaiheessa statuskomento näytti, että Apache2-palvelin olisi päällä. Yritin taas komentoa ````sudo a2ensite...```` ja ````sudo systemctl restart apache2````, mutta lopputulos oli sama kuin aiemmin. 

Komennolla ````sudo journalctl -xeu apache2.service```` sain seuraavat virheilmoitukset.

![kuva](https://github.com/user-attachments/assets/1fce2975-b0b2-41b8-9106-a4d889746341)

Error-logista löytyi komennolla ````sudo tail /var/log/apache2/error.log```` seuraava lokitieto. Ilmeisesti tämä syntyy, kun yritetään uudelleenkäynnistystä (Stack Overflow).

![kuva](https://github.com/user-attachments/assets/f3e25cd5-4ae9-4ffd-bd07-45192a9bdef7)

Käynnistin vielä virtuaalikoneen uudelleen ja annoin komennot ````sudo apt-get update```` ja ````sudo apt upgrade apache2````, jos päivityksistä olisi apua, mutta ei ollut. Lopetin työskentelyn tällä erää.

Jatkoin työskentelyä 16.2.2025 noin klo 10. Tilanne oli edelleen sama, eikä Apache käynnistynyt. Poistin sen komennolla ````sudo apt-get purge apache2````, hain päivitykset komennolla ````sudo apt-get update```` ja asensin uudelleen komennolla ````sudo apt-get install apache2````. Asennus onnistui ja Apachen status oli nyt aktiivinen.

Selasin eri keskustelupalstoja ja nettisivuja, ja sain niiden pohjalta idean, että virhe on suhteellisen yksinkertainen ja johtuu porteista. Sekä default-sivun että oman uuden sivuni VirtualHost-direktiivissä on portti 80, eikä näin voi olla. Hain tietoa, mitä toista porttia voisin käyttää, ja portti 8080 on ilmeisesti yleinen vaihtoehto portille 80 (SSL Insights).

Vaihdoin default-sivulle portin 8080, ja jätin omalle sivulle portin 80. Tietääkseni en tule käyttämään default-sivua tässä yhteydessä, joten ajattelin olevan pienempi riski toimivuuden kannalta vaihtaa numero sinne. Default-sivu ei myöskään ole "enabled" tällä hetkellä. 

![kuva](https://github.com/user-attachments/assets/3dd17729-e013-4a5e-89cc-c664ce2b9c6b)

Aluksi avasin palomuuriin reiän portille 8080, mutta pian sen jälkeen otin sen pois käytöstä komennolla ````sudo ufw deny 8080````, koska tajusin ettei tälle todennäköisesti ole tarvetta.

Nyt pystyin asettamaan sivun liljatatti.me käyttöön komennolla ````sudo a2ensite liljatatti.me```` ja komento ````sudo systemctl restart apache2```` toimi ongelmitta.

Aktiivista työskentelyaikaa kului tähän vaiheeseen noin kaksi tuntia.


### Tehtävä jatkuu 16.2.2025 klo 10.55

Yritin avata sivuston liljatatti.me selaimella, mutta se ei avautunut.

![kuva](https://github.com/user-attachments/assets/ed674e1c-f598-479f-9552-745e0626bc0f)

Tarkistin error-lokitiedot komennolla ````sudo tail /var/log/apache2/error.log````, ja sieltä ilmeni teksti "search permissions are missing on a component of the path". Ongelma on siis oikeuksissa.

![kuva](https://github.com/user-attachments/assets/06c43188-0703-4401-9afc-95d9f43cddf1) 

Käytin komentoa ````ls -l /path/to/file```` muodossa ````ls -l /etc/apache2/sites-available````, koska Name Based Virtual Host -tiedosto sijaitsee tässä polussa (Ask Ubuntu).

![kuva](https://github.com/user-attachments/assets/c7b3343d-351c-4a45-ac0e-893b8c5d00e6) 

Komento listasi nimenomaan tiedostojen oikeudet, sillä rivin ensimmäinen merkki ````-```` tarkoittaa tiedostoa. Ensimmäiset kolme merkkiä tämän jälkeen kuvaavat read, write and execute -lupia omistajalla, toiset kolme merkkiä ryhmällä ja viimeiset muilla ("ugo", user, group, other). (Ask Ubuntu.) "root root" viittaa tiedoston omistajaan ja ryhmään, jolla on pääsy tiedostoon.
(Linux Handbook). 

Ensin huomioni kiinnityi siihen, että kaikille sivuille on omistajuus rootilla. Haluan kuitenkin, että oman sivuni omistajuus on omalla käyttäjälläni, ja pystyn muokkaamaan tulevia kotisivuja normaalina käyttäjänä. Muistelin, että oppitunnilla käytettiin chown-komentoa, ja tarkensin sen oikeaa muotoa Copilotin tekoälyn avulla säästääkseni aikaa. Sain vastauksena kysymykseen "miten muutan tiedoston omistajuutta" komennon ````sudo chown yourusername:yourusername .````, ja pisteen kohdalle ohjeistettiin laittamaan kyseessä oleva tiedosto. Kokeilin siis komentoa ````sudo chown liljat.liljat liljatatti.me.conf````, ja se muutti oman sivuni omistajuuden omalle käyttäjälleni ja pääsyn ryhmälleni.

![kuva](https://github.com/user-attachments/assets/a38a7030-b386-441e-b514-dd5a76b6a7e2)

Nyt puuttui vielä loput oikeudet, sillä tällä hetkellä minulla oli oikeus vain lukea ja kirjoittaa, mutta ei ajaa. Käytin komentoa ````chmod +x liljatatti.me.conf```` ja sain execute-oikeudet. Käynnistin Apachen uudelleen.

![kuva](https://github.com/user-attachments/assets/ae4bd66d-6ab6-43bd-b464-7d05365e503a)

Tällä en vielä saanut sivustoa näkyviin, ja virheilmoitus error.logissa oli sama. Olin nyt antanut oikeuksia vain tiedostolle, ja koska ilmoitus kertoo ongelman liittyvän polkuun, kokeilin samaa komentoa kohdistettuna koko polkuun: ````chmod +x /home/liljat/````. Tämä onnistui.

![kuva](https://github.com/user-attachments/assets/9a241ffb-3801-46ae-a686-fed5a7bb6837)

Käynnistin Apachen uudelleen, ja sain nettisivulleni näkyviin aiemmin luomani index.html sivun sisällön. 

![kuva](https://github.com/user-attachments/assets/7d34027c-8a1a-403f-90c4-697c3fe7c0d6)

Aikaa tähän vaiheeseen kului 1,5 tuntia.


### Lähteet

Karvinen 2018, Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address: https://terokarvinen.com/2018/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/?fromSearch=name%20based. Luettu 15.2.2025.

Stack Overflow, Apache server keeps crashing, "caught SIGTERM, shutting down": https://stackoverflow.com/questions/1661802/apache-server-keeps-crashing-caught-sigterm-shutting-down. Luettu 15.2.2025.

SSL Insights, Port 80 vs 8080 vs 443: What’s the Difference?: https://sslinsights.com/port-80-vs-8080-vs-443/. Luettu 16.2.2025.

Ask Ubuntu, How do you view file permissions? https://askubuntu.com/questions/528411/how-do-you-view-file-permissions#528433. Luettu 16.2.2025.

Linux Handbook, Linux File Permissions and Ownership Explained with Examples: https://linuxhandbook.com/linux-file-permissions/. Luettu 16.2.2025. 


## c) Kotisivu, 16.2.2025 klo 15

Olin aloittanut etusivun tekemisen suoraan palvelimelle. Päätin luoda muut sivut ensin paikallisesti virtuaalikoneella, jotta voisin testata toimivuutta ennen sivujen julkaisemista. Localhost-sivuni oli hakemistossa ````/var/www/html/````, ja sieltä löytyi jo valmiiksi index.html-tekstitiedosto. Lisäsin hakemistoon kaksi muuta tiedostoa, portfolio.html ja cv.html. Loin HTML-sisältöä lyhyesti, ja muodostin linkit sivujen välille.

Kopioin nämä tiedostot virtuaalipalvelimelleni alla kuvassa näkyvällä komennolla. Tämä komento kopioi koko html-kansion kohteeseen, eli palvelimen kansioon ````publicsites````.

![kuva](https://github.com/user-attachments/assets/97a86e38-ac94-417b-9990-5b15dad96202)

Virtuaalipalvelimeni tiedostoihin muodostui uusi html-niminen hakemisto, josta kopioin tiedostot hakemistoon ````liljatatti.me```` komennoilla ````cp portfolio.html /home/liljat/publicsites/liljatatti.me```` ja ````cp cv.html /home/liljat/publicsites/liljatatti.me````.

![kuva](https://github.com/user-attachments/assets/bed80a30-fa9f-4c2d-9c4f-e97360c98c03)

Jotta sivujen linkit toimisivat oikein, piti vielä muokata tekstitiedostoihin oikeat osoitteet linkkien kohdalle. Avasin tiedoston nano-editorilla ja vaihdoin osoitteet jokaiseen tiedostoon osoittamaan haluttuun paikkaan.

![kuva](https://github.com/user-attachments/assets/9c923b57-6f7c-48ed-a600-079c902f76d8)

![kuva](https://github.com/user-attachments/assets/09bd1d35-1b71-49a5-a49f-58ff80ba412b)'

Kokeilin oman tietokoneeni selaimella toimivuutta, eikä ongelmia ilmennyt.

![kuva](https://github.com/user-attachments/assets/eeedd7ae-e894-4db4-ac91-4967179a96b9)

Testasin sivut vielä HTML-validaattorilla. Aluksi etusivulta sain saman huomautuksen kuin jossakin aiemmassa harjoituksessa, että kohdan ````<meta charset="utf-8" />```` kenoviiva on turha. Poistin sen heti kaikista tiedostoista, ja sen jälkeen kaikkien sivun tulos oli onnistunut, eli sivut ovat validia HTML5:a.

![kuva](https://github.com/user-attachments/assets/2a5a34d1-4dc4-48e8-a7be-d4e8afee0ef2)
![kuva](https://github.com/user-attachments/assets/b33e9528-3a10-4e22-807e-2b92eaeb34c8)
![kuva](https://github.com/user-attachments/assets/9e833a05-a4b2-4fbb-b4ef-4cf2df947cfd)



### Lähteet

Tehtävänanto h5, https://terokarvinen.com/linux-palvelimet/

## d) Alidomainit


## e) DNS-tiedot 
