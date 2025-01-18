# h1 Oma Linux

## x) Tiivistelmät

#### Raportin kirjoittaminen

- Raportin tulee olla toistettavissa samassa ympäristössä, jonka vuoksi myös harjoitusympäristö tulee raportoida
- Tekstin tulee olla helppolukuista sekä oikein ja imperfektissä kirjoitettua.
- Sisällön tulee olla täsmällistä, ja raportilla tulisi mainita yksityiskohtaisesti mm. annetut komennot ja tehdyt klikkaukset, kellonajat sekä kaikki viat ja odottamattomat tulokset.
- Lähdeviitteet ovat hyvän tavan mukaisia ja osoittavat, että aihealueeseen on perehdytty.
  
(Karvinen 2006)

#### What is Free Software?

- Vapaa ohjelmisto täyttää kaikki neljä vapautta ilman poikkeuksia
- Vapaus 0: Vapaus ajaa ohjelmia haluamallaan tavalla mihin tahansa tarkoitukseen, eli mikään ei kiellä tai estä ketään käyttäjää ajamasta ohjelmaa. Tämä vapaus ei tarkoita, että ohjelma olisi toimiva tai hyödyllinen käyttäjälle. Käyttäjällä on myös vapaus olla ajamatta ohjelmaa.
- Vapaus 1: Vapaus tutkia miten ohjelma toimii ja muuttaa sitä toimimaan haluamallaan tavalla edellyttää avointa lähdekoodia. Vapaus 1:een kuuluu myös vapaus käyttää omaa muutettua versiota alkuperäisen tilalla. Muutoksen ei tarvitse olla parannus. 
- Vapaus 2: Vapaus levittää kopioita eteenpäin ilman luvan kysymistä keneltäkään, joko ilmaiseksi tai maksua vastaan. Muutettuja ohjelmia ei ole pakko julkaista.
- Vapaus 3: Vapaus levittää kopioita omasta, muokatusta versiosta muille. Jos saatavilla, kopion tulisi sisältää ohjelman binaariset tai suoritettavat muodot, jotta ohjelma on kätevästi asennettavissa. Myös avoin lähdekoodi tulee olla.

(Free Software Foundation)

## a) Linuxin asentaminen

Tämä asennus suoritettiin lauantaina 18.1.2025 alkaen klo 14.30. Käytössäni oli oma itse koottu pöytäkone, jossa on Windows-käyttöjärjestelmä. Asennus tapahtui virtuaalikoneelle Oracle VirtualBoxissa, jonka asensin tietokoneelleni tätä tehtävää tehdessä.

#### Valmistelu

VirtualBox

Ensimmäisenä latasin Debian ISO kuvatiedoston cdimage.debian.org-nettisivulta, johon oli linkki kurssin ohjemateriaalissa (Karvinen 2024). Tarkistin, että tiedosto on live, versiota 12, tarkoitettu amd64:lle ja ympäristönä on xfce. Lataamani tiedosto oli siis debian-live-12.9.0-amd64-xfce.iso. 

#### Uuden koneen luominen

Valitsin aluksi VirtualBox Managerissa "Expert Mode". Uuden virtuaalikoneen luominen tapahtui painamalla yläpalkista "Machine" ja sieltä "New..."

VirtualBoxin näkymäni poikkesi ohjeen näkymästä. Pystyin antamaan normaalisti nimen uudelle koneelle ja valitsemaan tyypixi Linuxin ja versioksi Debian (64-bit), mutta aluksi ohjeessa ohjeistettu valinta "Skip Unattended Install" näkyi harmaana eikä sitä voinut valita. Lisäsin jo tässä vaiheessa ISO-imagen, ja sitten pystyin laittamaan valinnan myös tuohon ruutuun. 

![kuva](https://github.com/user-attachments/assets/d260e149-50b5-4989-8816-7d08f4fb8427)

Ohjeen mukaisesti muistia määriteltiin 4000 Mt, ja kovalevy valittiin luotavaksi heti. Sille annettiin kokoa 60 GB. Muuten oletusasetukset säilytettiin.

![kuva](https://github.com/user-attachments/assets/eec7aba9-3fb6-426f-a255-7d4c3df58a99)

![kuva](https://github.com/user-attachments/assets/9516886c-5f40-4066-8558-1b86a2bf900e)

#### Käynnistys

Virtuaalikoneen käynnistys tapahtui tuplaklikkaamalla uutta konetta otsikolla "Debian" VirtualBox Managerin vasemmassa laidassa. Taas näkymäni oli erilainen kuin ohjeessa, eikä päävalikkoon tullut vaihtoehtoa, jossa olisi ollut kernel.

![kuva](https://github.com/user-attachments/assets/eabee75c-663b-4b7a-8649-59f32d40ad85)

Tässä kohtaa poistin virtuaalikoneen noin klo 15 ja tein sen uudestaan siinä toivossa, että olisin tehnyt jotain väärin ensimmäisellä kerralla. Tein aiemmin kuvatut valinnat ja lopputulema oli sama. Päätin kuitenkin kokeilla toimivuutta valitsemalla Boot Menussa ensimmäisen valinnan "Live system (amd64). Näkymä oli hetken aikaa kokonaan musta mutta sen jälkeen työpöytä aukesi.

![kuva](https://github.com/user-attachments/assets/14270015-3c46-4baf-8a2b-ea94383b3e8f)

Avasin ylhäältä vasemmalta "Applications" ja sieltä "Web Browser", ja kokeilin hakusanaa "Tero Karvinen" ja pääsin nettisivulle.

![kuva](https://github.com/user-attachments/assets/69814421-a273-4d69-a71d-a0b49cd9a8ff)

#### Debianin asennus

Nyt kun hiiri ja verkko toimivat normaalisti ja näkymä oli oikea, aloitin Debianin asentamisen klikkaamalla "Install Debian" virtuaalikoneen työpöydällä. Kieleksi valitsin amerikanenglanti, sijainniksi Suomi ja näppäimistöksi suomenkielinen oletusnäppäimistö. Tässä kohtaa pystyi kokeilemaan skandikirjaimien toimintaa, ja ne toimivat kuten piti.

Partitions-kohdassa valitsin "Erase disk", Encrypt-kohdan jätin tyhjäksi, ja Boot loader location oli oletuksena valmiina "Master Boot Record..."

Users-kohdassa annoin käyttäjänimeksi oman nimeni, josta tuli automaattisesti lyhenne kirjautumisnimeksi. Tietokoneen nimeksi annoin saman "foo" joka ohjeessa mainittiin. Loin salasanan, ja automaattista kirjautumista koskeva valinta jätettiin tyhjäksi.

Lopuksi näkymä antoi koonnin valinnoista, ja sen jälkeen aloitin asennuksen painikkeesta "Install". Kun asennus oli valmis, näkymässä "All done" oli valmiina valinta "Restart now", jolloin painoin vain "Done" ja uudelleenkäynnistys alkoi.

![kuva](https://github.com/user-attachments/assets/f1420313-c4f9-4bfb-82f7-9aef52190d11)

#### Kirjautuminen

Uudelleenkäynnistyksen jälkeen näytölle tuli kirjautumisikkuna, johon annoin kirjautumistietoni. Kirjautuminen onnistui ja työpöytä aukesi sellaisenaan. 

![kuva](https://github.com/user-attachments/assets/4653640b-6dc6-4a77-80e5-e42a48e012af)


## Lähteet
Free Software Foundation. What is Free Software? Luettavissa: https://www.gnu.org/philosophy/free-sw.html#run-the-program. Luettu 18.1.2025.
Karvinen, Tero 2006: Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/. Luettu 18.1.2025.
https://terokarvinen.com/2021/install-debian-on-virtualbox/
