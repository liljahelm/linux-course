# h5 Nimekäs

## Alustus

Seuraavat harjoitukset on tehty virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

Virtuaalipalvelin on vuokrattu DigitalOcean-palvelusta, ja palvelimella on 1 Gt keskusmuistia ja 25 Gt:n SSD-levy.

Kaikki harjoitukset perustuvat tehtävänantoihin kevään 2025 Linux-palvelimet -kurssin sivustolla: https://terokarvinen.com/linux-palvelimet/.

## a) Nimi

Aloitin tehtävän tekemisen 15.2.2025 klo 14.

Tarkoituksena oli hankkia julkinen domainnimi ja laittaa se osoittamaan omaan virtuaalikoneeseen. Hankin itselleni domainnimen Namecheapin kautta, sillä sitä suositeltiin oppitunnilla ja GitHub Educationin kautta sai palveluun vuodeksi ilmaisen nimen. Hyödynsin rekisteröitymisessä tarvittavissa kohdissa Susanna Lehdon tehtäväraporttia ohjeena.

Ensin hakeuduin Namecheapin sivustolle GitHubin Student Developer Packin linkin kautta, ja rekisteröidyin. Rekisteröityminen piti tehdä henkilökohtaisella sähköpostilla koulun sähköpostin sijaan. Alussa valitsin kohdan GitHub Pages Free siltä varalta, jos haluaisin myöhemmin hyödyntää nettisivun tekemistä GitHubin repositoryjen kautta. 

![kuva](https://github.com/user-attachments/assets/95f2716d-4671-412e-9b4f-5577b95b587e)

Tilaus onnistui ja pääsin jatkamaan kirjautumiseen ja tarpeellisten toimien tekemiseen. Kirjauduttuani palveluun valitsin valikosta ````Domain List````, josta pääsin hallintapaneeliin ja valitsin valikon ````Advanced DNS````. Näkymässä oli aluksi viisi (kuvassa neljä) valmista tietuetta, jotka poistin, sillä niille ei ollut tässä yhteydessä käyttöä. 

![kuva](https://github.com/user-attachments/assets/07c06c77-57a1-4416-987b-3a025580e6a7)

Lisäsin kaksi uutta A-tietuetta (A Record) oman virtuaalipalvelimeni IP-osoitteella. A-tietue on yksi olennaisimmista DNS-tietueista, ja se yhdistää IP-osoitteen annettuun domainnimeen (Cloudfare). Host-kohtiin valitsin ````@```` ja ````www````, jotta sivulle pääsee sekä suoraan muodossa ````liljatatti.me```` että www-muodossa ````www.liljatatti.me````. TTL eli Time To Live -kenttään valitsin 5 min, eli DNS-palvelin saa säilyttää tietuetta viisi minuuttia ennen, kuin sitä pitää pyytää uudelleen, toisin sanoen päivittää. Lyhyt TTL-aika mahdollistaa DNS-tietojen nopean päivittymisen, mutta lisäävät verkkoliikennettä. (Wikipedia.) Lopuksi tallensin tiedot.

![kuva](https://github.com/user-attachments/assets/d643b5cf-bddb-4739-b1fe-430637d5a15e)

Viimeiseksi kokeilin hakea oman tietokoneeni selaimella sivua ````liljatatti.me````, ja sieltä avautui oikeasta IP-osoitteesta aiemmin luotu sivusto, eli tehtävä onnistui tältä osin.

![kuva](https://github.com/user-attachments/assets/a8692f67-f97b-4f26-a7f0-b334d18a49e0)

Aikaa tähän kului 20 minuuttia.

### Lähteet

Lehto 2022, Teoriasta käytäntöön pilvipalvelimen avulla (h4): https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/. Luettu 15.2.2025.

Cloudfare, DNS A record: https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/. Luettu 15.2.2025.

Wikipedia 2019, Time to Live: https://fi.wikipedia.org/wiki/Time_to_Live. Luettu 15.2.2025.


## b) Name Based Virtual Host

Aloitin tehtävän tekemisen 15.2.2025 klo 14.50.

Tarkoituksena oli saada Name Based Virtual Host näkykmään uudessa nimessä. Aloitin luomaan uutta Name Based Virtual Hostia virtuaalipalvelimen terminaalissa, koska ajattelin, että palvelimelle tulevat sivut tulisi luoda siellä. Sivun olisi voinut luoda myös paikallisesti ja sitten kopioida palvelimelle.

Loin uuden tiedoston komennolla ````/etc/apache2/sites-available/liljatatti.me.conf````, ja tarkistin tekstieditoriin lisäämäni sisällön cat-komennolla. Tässä vaiheessa sisältö vaikutti mielestäni virheettömältä, sillä olin huolellisesti tehnyt sen ohjeiden (Karvinen 2018) mukaan ja kopioinut tiedostopolut ilman kirjotusvirheitä.

### Ongelmatilanne

Jatkoin Tero Karvisen sivuston ohjeen mukaisesti ja annoin komennon ````sudo a2ensite liljatatti.me````, ja sen jälkeen komennon ````sudo systemctl restart apache2```` käynnistääkseni palvelimen uudelleen. Sain kuitenkin seuraavan kuvan mukaisen virheilmoituksen.

![kuva](https://github.com/user-attachments/assets/cf6cb48f-9006-4edf-a276-38c55647a3fc)

Annoin kehotuksen mukaisen komennon ````systemctl status apache2.service````, ja sieltä selvisi, ettei Apache2-palvelin ole aktiivisena. 

![kuva](https://github.com/user-attachments/assets/0be55294-efa7-40f3-afc5-9806b4fc8038)

En saanut palvelinta käynnistettyä komennolla ````systemctl start apache2````, vaan lopputulos oli sama kuin restart-komennon jälkeen. 

Jatkoin yrittämistä ja tässä kohtaa aloitin vaiheet alusta. Kokeilin poistua palvelimen terminaalista, ja palata uudelleen. Tässä vaiheessa statuskomento näytti, että Apache2-palvelin olisi päällä. Yritin taas komentoa ````sudo a2ensite...```` ja ````sudo systemctl restart apache2````, mutta lopputulos oli sama kuin aiemmin. 

Komennolla ````sudo journalctl -xeu apache2.service```` sain seuraavat virheilmoitukset.

![kuva](https://github.com/user-attachments/assets/1fce2975-b0b2-41b8-9106-a4d889746341)

Error-logista löytyi komennolla ````sudo tail /var/log/apache2/error.log```` seuraava lokitieto. Ilmeisesti tämä syntyy, kun yritetään uudelleenkäynnistystä (Stack Overflow).

![kuva](https://github.com/user-attachments/assets/f3e25cd5-4ae9-4ffd-bd07-45192a9bdef7)

Käynnistin vielä virtuaalikoneen uudelleen ja annoin komennot ````sudo apt-get update```` ja ````sudo apt upgrade apache2````, jos päivityksistä olisi apua, mutta ei ollut. Lopetin työskentelyn tällä erää.

Jatkoin työskentelyä 16.2.2025 noin klo 10. Tilanne oli edelleen sama, eikä Apache käynnistynyt. Poistin sen komennolla ````sudo apt-get purge apache2````, hain päivitykset komennolla ````sudo apt-get update```` ja asensin uudelleen komennolla ````sudo apt-get install apache2````. Asennus onnistui ja palvelimen status oli nyt aktiivinen.

Selasin eri keskustelupalstoja ja nettisivuja, ja sain niiden perusteella idean, että virhe saattaakin olla suhteellisen yksinkertainen ja johtuu porteista. Sekä default-sivun että oman uuden sivuni VirtualHost-direktiivissä on portti 80, eikä näin voi olla. Hain tietoa, mitä toista porttia voisin käyttää, ja portti 8080 on ilmeisesti yleinen vaihtoehto portille 80 (SSL Insights).

Vaihdoin default-sivulle portin 8080, ja jätin omalle sivulle portin 80. Tietääkseni en tule käyttämään default-sivua tässä yhteydessä, joten ajattelin olevan pienempi riski toimivuuden kannalta vaihtaa numero sinne. Default-sivu ei myöskään ollut "enabled" tällä hetkellä. 

![kuva](https://github.com/user-attachments/assets/3dd17729-e013-4a5e-89cc-c664ce2b9c6b)

Aluksi avasin palomuuriin reiän portille 8080, mutta pian sen jälkeen otin sen pois käytöstä komennolla ````sudo ufw deny 8080````, koska tajusin ettei tälle todennäköisesti ole tarvetta.

Nyt pystyin asettamaan sivun liljatatti.me käyttöön komennolla ````sudo a2ensite liljatatti.me```` ja komento ````sudo systemctl restart apache2```` toimi ongelmitta.

Aktiivista työskentelyaikaa kului tähän vaiheeseen noin kaksi tuntia.


### Tehtävä jatkuu

Jatkoin työskentelyä 16.2.2025 klo 10.55. Yritin avata sivuston liljatatti.me selaimella, mutta se ei avautunut.

![kuva](https://github.com/user-attachments/assets/ed674e1c-f598-479f-9552-745e0626bc0f)

Tarkistin error-lokitiedot komennolla ````sudo tail /var/log/apache2/error.log````, ja sieltä ilmeni teksti "search permissions are missing on a component of the path". Ongelma on siis oikeuksissa.

![kuva](https://github.com/user-attachments/assets/06c43188-0703-4401-9afc-95d9f43cddf1) 

Käytin komentoa ````ls -l /path/to/file```` muodossa ````ls -l /etc/apache2/sites-available````, koska Name Based Virtual Host -tiedosto sijaitsee tässä polussa, ja virheilmoituksen perusteella ongelma liittyy polun osioon (Ask Ubuntu). 

![kuva](https://github.com/user-attachments/assets/c7b3343d-351c-4a45-ac0e-893b8c5d00e6) 

Komento listasi nimenomaan tiedostojen oikeudet, mikä ilmenee rivin ensimmäisestä merkistä ````-````, joka tarkoittaa tiedostoa. Ensimmäiset kolme merkkiä tämän jälkeen kuvaavat read, write and execute -lupia omistajalla, toiset kolme merkkiä ryhmällä ja viimeiset muilla ("ugo", user, group, other). (Ask Ubuntu.) "root root" viittaa tiedoston omistajaan ja ryhmään, jolla on pääsy tiedostoon. (Linux Handbook.) 

Ensin huomioni kiinnityi siihen, että kaikkien sivujen omistajuus on rootilla. Haluan kuitenkin, että oman sivuni omistajuus on omalla käyttäjälläni, ja pystyn muokkaamaan tulevia kotisivuja normaalina käyttäjänä. Muistelin, että oppitunnilla käytettiin chown-komentoa, ja tarkensin sen oikeaa muotoa Copilotin tekoälyn avulla säästääkseni aikaa. Sain vastauksena kysymykseen "miten muutan tiedoston omistajuutta linuxissa" komennon ````sudo chown yourusername:yourusername .````, ja pisteen kohdalle ohjeistettiin laittamaan kyseessä oleva tiedosto. Kokeilin siis komentoa ````sudo chown liljat.liljat liljatatti.me.conf````, ja se muutti oman sivuni omistajuuden omalle käyttäjälleni.

![kuva](https://github.com/user-attachments/assets/a38a7030-b386-441e-b514-dd5a76b6a7e2)

Nyt puuttui vielä loput oikeudet, sillä tällä hetkellä minulla oli oikeus vain lukea ja kirjoittaa, mutta ei ajaa. Käytin komentoa ````chmod +x liljatatti.me.conf```` ja sain execute-oikeudet (Linux Handbook). Käynnistin Apachen uudelleen, jotta muutokset tulevat voimaan.

![kuva](https://github.com/user-attachments/assets/ae4bd66d-6ab6-43bd-b464-7d05365e503a)

Tällä toimenpiteellä en vielä saanut sivustoani näkyviin selaimella, ja virheilmoitus error.logissa oli sama kuin aiemmin. Olin nyt antanut oikeuksia vain tiedostolle, ja koska ilmoitus kertoo ongelman liittyvän polkuun, kokeilin samaa komentoa kohdistettuna koko polkuun: ````chmod +x /home/liljat/````. Tämä onnistui.

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


## c) Kotisivu

Aloitin tehtävän tekemisen 16.2.2025 klo 15. Tarkoituksena oli tehdä vähintään kolmen erillisen alasivun kotisivu. Sivujen muokkaamisen tulisi onnistua ilman pääkäyttäjän oikeuksia. 

Olin aikaisemmin aloittanut etusivun tekemisen suoraan palvelimelle. Päätin luoda muut sivut ensin paikallisesti virtuaalikoneella, jotta voisin testata toimivuutta ennen sivujen julkaisemista. Päätin hyödyntää localhost-sivuani, joka oli hakemistossa ````/var/www/html/````, ja sieltä löytyi jo valmiiksi index.html-tekstitiedosto. Lisäsin hakemistoon kaksi muuta tiedostoa, portfolio.html ja cv.html. Loin HTML-sisältöä lyhyesti kaikille sivuille, ja muodostin linkit sivujen välille. Testasin toimivuutta paikallisella selaimella avaamalla sivut osoitteissa ````localhost````, ````localhost/portfolio.html```` ja ````localhost/cv.html````, ja kaikki toimi toivotusti.

Kopioin tiedostot virtuaalipalvelimelleni alla olevassa kuvassa näkyvällä komennolla. Tämä komento kuitenkin kopioi koko html-kansion kohteeseen, eli virtuaalipalvelimen kansioon ````publicsites````.

![kuva](https://github.com/user-attachments/assets/97a86e38-ac94-417b-9990-5b15dad96202)

Virtuaalipalvelimeni tiedostoihin siis muodostui uusi html-niminen hakemisto, josta sitten kopioin itse tiedostot hakemistoon ````liljatatti.me```` komennoilla ````cp portfolio.html /home/liljat/publicsites/liljatatti.me```` ja ````cp cv.html /home/liljat/publicsites/liljatatti.me````.

![kuva](https://github.com/user-attachments/assets/bed80a30-fa9f-4c2d-9c4f-e97360c98c03)

Jotta sivujen linkit toimisivat oikein, piti vielä muokata tekstitiedostoihin oikeat osoitteet linkkien kohdalle. Avasin tiedoston nano-editorilla ja vaihdoin osoitteet jokaiseen tiedostoon osoittamaan haluttuun paikkaan.

![kuva](https://github.com/user-attachments/assets/9c923b57-6f7c-48ed-a600-079c902f76d8)

![kuva](https://github.com/user-attachments/assets/09bd1d35-1b71-49a5-a49f-58ff80ba412b)'

Kokeilin oman tietokoneeni selaimella toimivuutta, eikä ongelmia ilmennyt.

![kuva](https://github.com/user-attachments/assets/eeedd7ae-e894-4db4-ac91-4967179a96b9)

Testasin sivut vielä HTML-validaattorilla. Aluksi etusivu ````liljatatti.me```` aiheutti aiemmista harjoituksista tutun huomautuksen, että kohdan ````<meta charset="utf-8" />```` kenoviiva on turha. Poistin sen heti kaikista tiedostoista, ja sen jälkeen kaikkien sivun tulos oli onnistunut, eli sivut ovat validia HTML5:a.

![kuva](https://github.com/user-attachments/assets/2a5a34d1-4dc4-48e8-a7be-d4e8afee0ef2)
![kuva](https://github.com/user-attachments/assets/b33e9528-3a10-4e22-807e-2b92eaeb34c8)
![kuva](https://github.com/user-attachments/assets/9e833a05-a4b2-4fbb-b4ef-4cf2df947cfd)

Aikaa tähän vaiheeseen kului 45 minuuttia.

### Lähteet

Tehtävänanto h5: https://terokarvinen.com/linux-palvelimet/

HTML-validator: https://validator.w3.org


## d) Alidomainit

Aloitin tehtävän tekemisen 16.2.2025 klo 16.15. Tarkoituksena oli luoda kaksi uutta alidomainia, jotka avaavat saman pääsivun, kuin päädomain.

Siirryin kirjautumaan Namecheapiin ja avasin sieltä Domain listin ja oman domainin kohdalta ````Manage```` ja ````Advanced DNS````. Lisäsin uuden tietueen alla näkyvillä tiedoilla ja tallensin. Käytin apuna Namecheapin omilla sivuilla olevaa ohjetta. (Namecheap 2024.)

![kuva](https://github.com/user-attachments/assets/ddfe9cec-34d0-4f5e-ac23-c7e251784a65)

Hain oman tietokoneeni Firefox-selaimella ````linuxkurssi.liljatatti.me````, mutta sivua ei löytynyt. Loin myös toisen tietueen ````blog````, mutta sekään ei auennut. Päätin pitää taukoa ja odottaa hetken, jos päivittymisessä menisi hieman aikaa.

Aikaa tässä vaiheessa oli kulunut 15 minuuttia.

Klo 17 jatkoin tehtävän tekemistä. Oman tietokoneeni tai virtuaalikoneen Firefox- tai Google Chrome -selaimilla sivut eivät auenneet, mutta oman puhelimeni DuckDuckGo-sovelluksen selain avasi molemmilla alidomaineilla etusivuni onnistuneesti.

![kuva](https://github.com/user-attachments/assets/0280fc4c-debb-44c1-9101-585bccb82f09)

![kuva](https://github.com/user-attachments/assets/314811aa-2fe7-4876-90fc-ce3019d6029c)

Tässä kohtaa jäi selvittämättä, mistä viive omalla tietokoneella ja virtuaalikoneella voisi johtua. 

Maanantaina 17.2.2025 klo 13 alidomainit aukesivat myös oman tietokoneeni Firefox-selaimella.

![kuva](https://github.com/user-attachments/assets/6f91335a-605e-4303-89d8-116d1a2e7c95)


### Lähteet
Namecheap 2024, How to Create a Subdomain for my Domain: https://www.namecheap.com/support/knowledgebase/article.aspx/9776/2237/how-to-create-a-subdomain-for-my-domain/. Luettu 16.2.2025.



## e) DNS-tiedot

Aloitin tehtävän tekemisen 17.2.2025 klo 13.35. Tarkoituksena oli tutkia kolmen eri nimen DNS-tietoja host ja dig -komennoilla.

### Host

Ennen kuin host-komennot toimivat, host piti asentaa. Hain saatavilla olevat päivitykset komennolla ````sudo apt-get update```` ja asensin hostin komennolla ````sudo apt-get install host````.

Host-komentoa käytetään kääntämään verkkotunnus IP-osoitteeksi ja päinvastoin (komento man host). Annoin komennon host kolmen eri nimen kanssa, jotka olivat oma domainnimeni ````liljatatti.me````, pienen yhdistyksen nettisivu ````helsingintarmo.fi```` ja suuri, kaikkien tuntema toimija ````google.com````.

![kuva](https://github.com/user-attachments/assets/6627addd-8052-46e6-8cac-fd3a87c94d65)

Oma domainnimeni antoi tuloksena virtuaalipalvelimeni IP-osoitteen. Muita yksityiskohtia oli sähköpostin välittämiseen liittyvät tiedot. Nämä löytyivät myös Namecheapin sivulta oman domainnimeni Advanced DNS -asetuksista.

![kuva](https://github.com/user-attachments/assets/b5eaf6df-51c7-4a5b-813a-91eb4e73aedf)

Helsingin Tarmon osalta tiedot olivat samankaltaiset, eli palvelimen IP-osoite ja sähköpostitietoja.

Googlen kohdalla näkyvissä oli muiden jo tarkasteltujen tietojen lisäksi myös heidän palvelimensa IPv6-osoite. Ilmeisesti omalla virtuaalipalvelimellani tai Helsingin Tarmon palvelimella ei siis ole IPv6-osoitteita käytössä. Kirjauduin DigitalOceaniin ja tarkistin asian, ja näin tosiaan oli. Katsoin, mitä IPv6-osoitteen käyttöönotto vaatisi, ja olemassa olevan dropletin kohdalla se vaatii manuaalista konfigurointia. Sivustolla oli selkeän oloinen ohje (DigitalOcean 2024), mutta en vielä ryhtynyt toimeen.

Aikaa kului 15 minuuttia.

### Dig

Aloitin tehtävän tekemisen 17.2.2025 klo 14. Myös dig piti ensin asentaa komennolla ````sudo apt-get install dnsutils```` (Ask Ubuntu).

Dig on työkalu, joka hakee DNS-tietoja ja näyttää vastaukset nimipalvelimelta, johon kysely tehtiin. Dig-komentoa käytetään mm. vianselvitykseen. Komentoon on mahdollista liittää useita eri avainsanoja, jotka vaikuttavat hakujen tekotapaan ja lopputulokseen. Nämä liitetään pääkomentoon plus-merkillä. Avainsanat voivat mm. asettaa tai nollata valinnan, tai antaa arvoja valinnoille. Monen valinnan tiedot, kuten ````+additional```` ja ````+answer```` näytetään oletuksena. Jos haluaa nähdä enemmän tietoa, voi käyttää esimerkiksi avainsanaa ````+all````, joka asettaa kaikki näyttöön liittyvät asetukset päälle. (komento man dig.)

#### dig liljatatti.me

Testasin aluksi komentoja ````dig liljatatti.me```` ja ````dig liljatatti.me +all````. Huomasin, että tulostetut tiedot olivat lähes täysin samat.

![kuva](https://github.com/user-attachments/assets/efbf229a-4513-4379-a126-f782e627844b) 

![kuva](https://github.com/user-attachments/assets/d0bef996-c09d-4cca-bba8-c8a99ffd483b) 

Ensimmäinen rivi näyttää asennetun digin version ja domainnimen, johon kysely kohdistui. Seuraavilla riveillä on tietoa globaaleista vaihtoehdoista ja teknistä tietoa vastauksesta, kuten statuksen, joka tässä tapauksessa on NOERROR, eli ei virheitä kyselyssä. (Linuxize 2020.) OPT PSEUDOSECTION liittyy DNS:n laajennuksiin (Wikipedia 2024). Kysymysosio (QUESTION SECTION) näyttää itse kyselyn, joka on oletuksena pyyntö A-tietueeseen, kuten myös tässä tapauksessa kirjaimesta "A" ilmenee (Linuxize 2020). 

Vastausosiossa (ANSWER SECTION) nähdään kyselyn vastaus. Ensimmäisenä asiana vastauksesta huomaan, että domain ````liljatatti.me```` osoittaa IP-osoitteeseen 178.62.233.5, joka on virtuaalipalvelimeni osoite. A-kirjain viittaa A-tietueeseen, eli domainnimi yhdistettiin oikeaan IP-osoitteeseen (Cloudfare a). Toinen kohta rivillä ilmaisee TTL-ajan (Time To Live) sekunneissa, eli kauanko tietue säilyy välimuistissa ennen päivittämistä. Ensimmäisen haun kohdalla lukema on 300, eli viisi minuuttia, kuten alussa määrittelin. Toisen haun kohdalla muutamia sekunteja oli kulunut, ja lukema oli nyt 293. ````IN````-kohta tarkoittaa luokkaa, tässä yhteydessä Internetiä. (Linuxize 2020, Stackoverflow a.)

Viimeinen osio kertoo kyselyyn liittyvää statistiikkaa, kuten kyselyn ajankohdan, kuluneen ajan, viestin koon ja palvelimen IP-osoitteen.

Lähdesivullani (Linuxize) oli tuotu esille myös kohdat ````AUTHORITY```` ja ````ADDITIONAL````, joita omassa haussani ei ollut näkyvissä. En saanut lisää tuloksia avainsanoilla ````+additional```` tai ````+authority````. Kokeilin tehdä haun komennolla ````dig 178.62.233.5 liljatatti.me```` ja sain Authority-osion jokseenkin näkyviin (Serverfault). 

![kuva](https://github.com/user-attachments/assets/8a31267f-c95f-46b8-925f-31a6f1357b45)

Komennolla ````dig liljatatti.me +trace```` sain authority-osioon paljon lisätietoa, alla kuvassa ote viimeisistä riveistä. Lyhenne ````NS```` riveillä tarkoittaa name serveriä eli nimipalvelinta, joka vastaa kyseisestä domainista (Cloudfare b). Sain siis näkyviin domainista vastaavan nimipalvelimen, ````dns2 ja dns1.registrar-servers.com````. ````Registrar-servers```` oli myös host-komennon tulosteessa sähköpostitiedoissa.

![kuva](https://github.com/user-attachments/assets/fad2ae65-86ac-4f1b-96f6-d00fe058d358)

Tässä vaiheessa en saanut Additional-osiota näkyviin. Alkuperäisessä haussani kohdassa ````flags```` kuitenkin näkyi additional-osio numerolla 1, eli tulkintani mukaan se on haettu. Mahdollisesti mitään näytettävää tietoa ei siis ollut?

Törmäsin myöhemmin keskustelupalstalla vielä komentoon ````dig domainname ANY````, ja kokeilin sitä. Tulos oli lähes sama kuin aikaisemmin.

![kuva](https://github.com/user-attachments/assets/b5fd0369-126b-462b-9051-1a667438ba71)

Ilmeisesti komento ei aina näytä kaikkia lisätietoja, sillä hakutulos riippuu siitä, mitä DNS-palvelimia haussa käytetään. (Serverfault b.) Ilmeisesti tietoa ei tule juurikaan lisää, koska tulokset haetaan virtuaalikoneen(?) omasta välimuistista, eikä haussa käytetä autoritatiivista nimipalvelinta, joka hakisi vastauksen (Superuser).

Tähän kului aikaa 2 tuntia. 


#### dig helsingintarmo.fi

Jatkoin tehtävää 22.2.2025 klo 9.55. Käytin komentoa ````dig helsingintarmo.fi````. Lähes kaikki tiedot alkupuolella olivat samoja, kuin edellisessä haussa. TTL-aika oli kuitenkin pidempi, 600 sekuntia, ja luonnollisesti domainin IP-osoite oli eri. 

![kuva](https://github.com/user-attachments/assets/3418af52-c065-433b-a381-1a8ede9d158b)

Hain nimipalvelinten tietoja komennolla ````dig helsingintarmo.fi +trace````, ja hakutuloksena nousi mm. host-osiossa näkynyt ````euronic````. Euronic vastaa siis tämän domainin DNS- ja sähköpostitiedoista.

![kuva](https://github.com/user-attachments/assets/e8a5bbdb-b543-4208-ae0f-f95ed3160d36)

Komennolla ````dig helsingintarmo.fi ANY```` sain hieman enemmän tietoa. Vastauksia löytyi 8, ja tällä kertaa tietueita oli erilaisia, joista uusia olivat TXT, MX ja SOA. Myös tämä haku olisi näyttänyt ````+trace```` haussa löytyneet nimipalvelinten tiedot.

![kuva](https://github.com/user-attachments/assets/bf3b9f66-473e-4b5a-a2fc-0dbda905a4ef)

TXT-tietue tarkoittaa, että domainin ylläpitäjä saa syöttää tekstiä DNS-järjestelmään. Syötetyn tekstin tunnistaa lainausmerkistä, kuten kuvassakin näkyy. Tietue on tavallaan muistiinpano. TXT-tietue voi auttaa ehkäisemään roskapostia sähköpostissa, sillä sitä käytetään sähköpostipalvelimilla tunnistamaan, onko lähde luotettu. Se voi myös auttaa varmentamaan domainin omistajuuden, jos tietuetta käytetään todistamaan, että tietty taho pystyy tekemään muutoksia siihen. (Cloudfare c.)

MX-tietue eli "mail exchange" ohjaa sähköpostiviestin sähköpostipalvelimelle. MX-tietue kertoo, miten viesti pitäisi reitittää SMTP-protokollan mukaan. Tietue sisältää prioriteettinumeron, joka omassa haussani oli 10. MX-tietueita oli kaksi, joista ensimmäisen arvossa on numero 1 ````mx-in1.euronic.fi````, ja toisessa numero 2 ````mx-in2.euronic.fi````. Molemmissa oli prioriteettinumero 10, joten ne ovat tärkeysjärjestyksessä tasaveroiset ja vastaanottavat kumpikin saman verran sähköposteja. Mitä alhaisempi prioriteettinumero on, sitä korkeampi prioriteetti on. (Cloudfare d.)

SOA-tietue eli "start of authority" löytyy jokaiselta DNS-alueelta, joka täyttää IETF:n standardit. Tietue sisältää tietoa mm. domainin tai alueen ylläpitäjän sähköpostiosoitteesta ja viimeisistä päivitysajoista. Omassa hakutuloksessani mainittiin jälleen euronic, joka löytyi aikaisemmistakin kohdista sähköpostitietoihin liittyen. Perässä olevat numerosarjat kertovat sarjanumeron, päivitysajan, aikamääreen kauanko odotetaan uusia päivityksiä, milloin kyselyihin vastaaminen lopetetaan, ja TTL-ajan. (Cloudfare e.) Kuvassa näkyvät nämä kaikki numerot ja sekuntimäärät.

Aikaa kului 50 minuuttia.


#### dig google.com 

Aloitin tämän tehtävän 23.2.2025 klo 14.15. Käytin haun tekemiseksi komentoa ````dig google.com ANY````. Hakutuloksia löytyi 9, ja uusina termeinä tietueiden sarakkeesta löytyi ````AAAA```` ja ````HTTPS````.

![image](https://github.com/user-attachments/assets/47c1625c-312e-4576-a187-b715ec261861)

AAAA-tietue yhdistää domainnimen IPv6-osoitteen kanssa, kuten A-tietue tekee IPv4-osoitteen kanssa (Cloudfare f). Kuten host-hakujen kanssa kävi ilmi, näistä kolmesta osoitteesta vain googlella oli käytössä IPv6-osoitteet, jonka vuoksi edellisten dig-komentojen tuloksena ei ilmennyt vielä AAAA-tietueita. Vastauksen IPv6-osoite täsmäsi host-komennolla löytyneeseen osoitteeseen.
Myös MX-tietueen prioriteettinumero 10 ja osoite täsmäsi host-komennon vastaukseen.

HTTPS-tietueen rivin arvo kertoo ensin prioriteetin, joka tässä tapauksessa on numero 1. Piste ilmaisee hostin, jos se on sama kuin haettu domainnimi. ````alpn="h2,h3"```` tarkentaa protokollan version, tässä tapauksessa siis HTTP2 ja HTTP3 -protokollat ovat mahdollisia käyttää. HTTPS-tietue tarjoaa tietoa turvallisen yhteyden muodostamiseen, ja sen käyttö vähentää yhteyden muodostamisessa pyyntöjen määrää, mikä nopeuttaa ja sujuvoittaa yhteyden muodostamista. (Gcore docs.)

Kuten edellisessä tehtävävaiheessa, tässäkin haussa saatiin tuloksena SOA eli start of authority, joka ilmaisee tietoa ylläpitäjästä. NS-tietueita löytyi neljä, eli enemmän kuin edellisissä hauissa, mikä vaikuttaa loogiselta, kun kyseessä on iso toimija. 

Aikaa kului 40 minuuttia.


#### Kaiken kaikkiaan tehtäviin kului aikaa noin 8,5 tuntia, johon ei ole laskettu raportin muotoiluun kulunutta aikaa.

### Lähteet

Terminaalikomento man host, luettu 17.2.2025.

DigitalOcean 2024, How to Enable IPv6 on Droplets: https://docs.digitalocean.com/products/networking/ipv6/how-to/enable/#on-existing-droplets. Luettu 17.2.2025.

Ask Ubuntu, How do I install dig? https://askubuntu.com/questions/25098/how-do-i-install-dig. Luettu 17.2.2025.

Terminaalikomento man dig, luettu 17.2.2025.

Linuxize 2020, Dig Command in Linux (DNS Lookup): https://linuxize.com/post/how-to-use-dig-command-to-query-dns-in-linux/. Luettu 17.2.2025.

Wikipedia 2024, Extension Mechanisms for DNS: https://en.wikipedia.org/wiki/Extension_Mechanisms_for_DNS. Luettu 17.2.2025.

Cloudfare a, DNS A record: https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/. Luettu 17.2.2025.

Stackoverflow, Meaning of the five fields of the ANSWER SECTION in dig query: https://stackoverflow.com/questions/20297531/meaning-of-the-five-fields-of-the-answer-section-in-dig-query. Luettu 17.2.2025.

Serverfault a, Why does dig not show the authority section and how to make it show the authoritative name servers that hold the DNS query`s answer? https://serverfault.com/questions/1088257/why-does-dig-not-show-the-authority-section-and-how-to-make-it-show-the-authorit. Luettu 17.2.2025.

Cloudfare b, DNS NS record: https://www.cloudflare.com/learning/dns/dns-records/dns-ns-record/. Luettu 17.2.2025.

Serverfault b, List all DNS records in a domain using dig? https://serverfault.com/questions/138949/list-all-dns-records-in-a-domain-using-dig. Luettu 22.2.2025.

Superuser, 'dig any' results wrong, missing data: https://superuser.com/questions/184066/dig-any-results-wrong-missing-data. Luettu 22.2.2025.

Cloudfare c, What is a DNS TXT record? https://www.cloudflare.com/learning/dns/dns-records/dns-txt-record/. Luettu 22.2.2025.

Cloudfare d, What is a DNS MX record? https://www.cloudflare.com/learning/dns/dns-records/dns-mx-record/. Luettu 22.2.2025.

Cloudfare e, What is a DNS SOA record? https://www.cloudflare.com/learning/dns/dns-records/dns-soa-record/. Luettu 22.2.2025.

Cloudfare f, DNS AAAA record: https://www.cloudflare.com/learning/dns/dns-records/dns-aaaa-record/. Luettu 23.2.2025.

Gcore docs, What is an HTTPS record and how is it configured? https://gcore.com/docs/dns/dns-records/what-is-an-https-record-and-how-is-it-configured. Luettu 23.2.2025.

