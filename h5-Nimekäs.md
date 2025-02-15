# h5 Nimekäs

## Alustus

Seuraavat harjoitukset on tehty virtuaalikoneella, johon asennettiin käyttöjärjestelmäksi Debian 64-bit. Virtuaalikoneella on 60 Gt virtuaalinen kovalevy ja muistia 4000 Mt. Virtual memoryä lisättiin 128 megatavuun, jonka jälkeen käyttö nopeutui. Käytän virtuaalikonetta VirtualBoxilla (versio 7.1.4) omalla itse kootulla pc:llä (käyttöjärjestelmä Windows 11 64-bit, prosessori Intel i3-9100F, näytönohjain Asus GTX 1060 3GB, emolevy ASRock H310CM-HDV, RAM 8 GB, SSD 500GB).

Kaikki harjoitukset perustuvat tehtävänantoihin kevään 2025 Linux-palvelimet -kurssin sivustolla: https://terokarvinen.com/linux-palvelimet/.

## a) Nimi, 15.2.2025 klo 14

Hankin itselleni domainnimen NameCheapin kautta, sillä sitä suositeltiin oppitunnilla ja GitHub Educationin kautta sai palveluun vuodeksi ilmaisen nimen. Hakeuduin NameCheapin sivustolle GitHubin Student Developer Packin linkin kautta, ja rekisteröidyin. Rekisteröityminen piti tehdä henkilökohtaisella sähköpostilla koulun sähköpostin sijaan. Alussa valitsin kohdan GitHub Pages Free siltä varalta, jos haluaisin myöhemmin hyödyntää nettisivun tekemistä GitHubin repositoryjen kautta. 

![kuva](https://github.com/user-attachments/assets/95f2716d-4671-412e-9b4f-5577b95b587e)

Tilaus onnistui ja pääsin jatkamaan kirjautumiseen ja tarpeellisten toimien tekemiseen. Kirjauduttuani palveluun valitsin valikosta ````Domain List````, josta pääsin hallintapaneeliin ja valitsin valikon ````Advanced DNS````. Näkymässä oli aluksi viisi (kuvassa neljä) valmista tietuetta, jotka poistin, sillä niistä ei ole tässä yhteydessä hyötyä. 

![kuva](https://github.com/user-attachments/assets/07c06c77-57a1-4416-987b-3a025580e6a7)

Lisäsin kaksi uutta A-tietuetta (A Record) oman virtuaalipalvelimeni IP-osoitteella. A-tietue on yksi olennaisimmista DNS-tietueista, ja se yhdistää IP-osoitteen annettuun domainnimeen (Cloudfare). Host-kohtiin valitsin "@" ja "www", jotta sivulle pääsee sekä suoraan muodossa ````liljatatti.me```` että www-muodossa ````www.liljatatti.me````. TTL eli Time To Live -kenttään valitsin 5 min, eli DNS-palvelin saa säilyttää tietuetta viisi minuuttia ennen, kuin sitä pitää pyytää uudelleen. Lyhyt TTL-aika mahdollistaa DNS-tietojen nopean päivittymisen, mutta lisäävät verkkoliikennettä. (Wikipedia.) Lopuksi tallensin tiedot.

![kuva](https://github.com/user-attachments/assets/d643b5cf-bddb-4739-b1fe-430637d5a15e)

Viimeiseksi kokeilin hakea oman tietokoneeni selaimella sivua ````liljatatti.me````, ja sieltä avautui oikeasta IP-osoitteesta aiemmin luotu sivusto.

![kuva](https://github.com/user-attachments/assets/a8692f67-f97b-4f26-a7f0-b334d18a49e0)

Aikaa tähän kului 20 minuuttia.

### Lähteet

Lehto 2022, Teoriasta käytäntöön pilvipalvelimen avulla (h4): https://susannalehto.fi/2022/teoriasta-kaytantoon-pilvipalvelimen-avulla-h4/. Luettu 15.2.2025.

Cloudfare, DNS A record: https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/. Luettu 15.2.2025.

Wikipedia 2019, Time to Live: https://fi.wikipedia.org/wiki/Time_to_Live. Luettu 15.2.2025.


## b) Name Based Virtual Host nimeen


## c) Kotisivu


## d) Alidomainit


## e) DNS-tiedot 
