# Mamma Mia Sombor — QR cenovnik

Gotov HTML/CSS/JavaScript/PHP projekat, bez baze podataka, Composer-a, npm-a ili obaveznih spoljašnjih servisa. Jelovnik se ispisuje na serveru i dostupan je i bez JavaScript-a. JavaScript dodaje pretragu, filtere i generisanje QR koda.

## Brzo pokretanje

1. Raspakujte ZIP.
2. Na hosting sa PHP 8.2 ili novijim kopirajte sadržaj foldera `mamma-mia-qr` u `public_html/cenovnik/`. Za lokalni razvoj pokrenite `php -S localhost:8080` iz tog foldera i otvorite `http://localhost:8080/`.
3. Otvorite javnu adresu, npr. `https://VAS-DOMEN.rs/cenovnik/`, i proverite svih šest jezika. `VAS-DOMEN` je primer, a ne adresa za štampu.
4. Otvorite `https://VAS-DOMEN.rs/cenovnik/qr.php`, unesite **stvarnu javnu HTTPS adresu početne stranice**, pa generišite kod.
5. Preuzmite SVG ili odštampajte karticu iz pregledača. Skenirajte kod telefonom pre štampe većeg tiraža. Za sto koristite kod približno 4 × 4 cm ili veći, crn na belom, bez uklanjanja bele margine.

QR kod ne zahteva pretplatu i ne ističe. Promene cena ne menjaju QR kod. Ako promenite adresu, potrebno je sačuvati preusmerenje sa stare adrese ili odštampati novi kod. `qr.php` ne menja podatke na serveru.

## Struktura

```text
mamma-mia-qr/
  index.php                 početni izbor jezika
  menu.php                  serverski prikaz menija
  qr.php                    lokalni QR generator i kartica za štampu
  data/
    menu.json               sve stavke, prevodi, zapremine i cene
    translations.json       prevodi interfejsa i veličina
    site.json               kontakt, dostava, opciona javna adresa
  includes/bootstrap.php    zajedničko učitavanje i HTML zaštita
  assets/
    style.css               responsive dizajn i štampa
    app.js                  pretraga i filteri
    qr.js                   validacija adrese i SVG preuzimanje
    img/                    lokalni logo i fotografija sa postojećeg sajta
    vendor/                 lokalna QR biblioteka i MIT licenca
  references/               tri izvorne fotografije za proveru unosa
  tests/validate.php         provera podataka
  PROVERA.md                izveštaj o izvršenim proverama
  IZVORI.md                 poreklo sadržaja i napomene o prepisu
```

## Dodaci i ruski jezik — izmena od 27.10.2026.

Kategorija `extras` (Dodaci) pripada hrani i stoji iza svih ostalih jela, pre pića. Sadrži 19 dodataka prema novoj fotografiji `references/dodaci.jpeg` i korisnikovoj dopuni. Umesto reda „Palačinke” sa te fotografije dodate su dve zasebne stavke: „Džem” za 80 dinara i „Orasi” za 80 dinara. Postojeće jelo Palačinke za 250 dinara ostaje u jelovniku. Za masline (10), jaje (30) i feferonu (20) cena važi po komadu, uz `label: "piece"`. Sve cene su u dinarima (RSD).

Ruski (`ru`) dostupan je na početnoj stranici i u navigaciji, sa prevodima svih stavki, sastojaka i interfejsa. Projekat sada ima 95 stavki, 145 cenovnih varijanti, 13 kategorija i šest jezika. Za ažuriranje postojećeg hostinga zamenite sadržaj projekta novom verzijom; ako ste menjali kontakt ili javni URL, sačuvajte svoj `data/site.json`. QR kod ostaje isti ako adresa cenovnika ostaje ista.

## Promena cene

Otvorite `data/menu.json` u UTF-8 editoru, pronađite stavku po `id` ili nazivu, promenite odgovarajući `price` i sačuvajte. Cena je ceo broj u dinarima, bez navodnika, oznake valute i separatora hiljada. Cene su zajedničke svim jezicima, pa menjate samo jedno mesto. Na primer:

```json
{
  "label": "",
  "volume": "0.25 l",
  "price": 175
}
```

Ovo je jedna varijanta stavke `bavaria-draft`. Druga varijanta je `0.5 l`, `350`. Obrišite keš pregledača ili osvežite stranicu da vidite novu cenu.

## Dodavanje stavke

U `items` niz fajla `data/menu.json` dodajte objekat kao u primeru. Pazite na zareze između objekata; JSON ne dozvoljava komentare ni zarez posle poslednjeg elementa.

```json
{
  "id": "novi-sok",
  "category": "soft",
  "name": {
    "sr": "Sok od jabuke",
    "en": "Apple juice",
    "hu": "Almalé",
    "de": "Apfelsaft",
    "es": "Zumo de manzana",
    "ru": "Яблочный сок"
  },
  "description": {"sr":"", "en":"", "hu":"", "de":"", "es":"", "ru":""},
  "variants": [{"label":"", "volume":"0.2 l", "price":220}]
}
```

`id` mora biti jedinstven, sa malim latiničnim slovima, ciframa i crticama. `category` mora odgovarati `id` vrednosti neke kategorije. `name` i `description` imaju svih šest ključeva: `sr`, `en`, `hu`, `de`, `es`, `ru`. Prazan opis je dozvoljen. Brendovi i tradicionalni nazivi pica ostaju prepoznatljivi u svim jezicima; opisni nazivi i svi sastojci su prevedeni.

Za više veličina dodajte objekte u `variants`. `label` može biti prazan ili `mini`, `standard`, `maxi`, `gala`, `portion`, `piece`. `volume` sadrži zapreminu ili prečnik, npr. `0.33 l` ili `27 cm`; može biti prazan kada mera nije navedena. Oznaka `piece` prevodi se kao „po komadu”. Nazivi veličina prevode se preko `translations.json`. Za novu kategoriju dodajte objekat u `categories`, sa jedinstvenim `id`, `group` (`food` ili `drinks`) i šest prevoda u `name`. Redosled kategorija i stavki u JSON-u određuje redosled na stranici.

Posle izmene pokrenite `php tests/validate.php`. Provera će ukazati na neispravan JSON, duple ID-jeve, nedostajuće prevode, nevažeću kategoriju ili cenu. Završne provere konkretnih cena predstavljaju početni referentni unos; ažurirajte ih ako namerno menjate te cene. Zatim vizuelno proverite izmenjenu stavku.

## Kontakt i podešavanja

`data/site.json` sadrži adresu, telefone, cenu dostave i `publicUrl`. Broj za poziv u `href` upisuje se u međunarodnom formatu `+381…`, bez razmaka. `publicUrl` je početno prazan kako se ne bi štampao QR za nepotvrđenu adresu. Ako tu upišete konačnu adresu sa HTTPS-om, ona će biti unapred popunjena u QR alatu. Generator se pokreće dugmetom.

## Hosting

- PHP 8.2+ sa standardnom JSON podrškom; nije potrebna baza podataka. Preporuka: trenutno podržana PHP verzija koju nudi hosting.
- Apache 2.4: uključeni `.htaccess` fajlovi postavljaju `index.php` kao početni dokument, gase listing foldera i blokiraju direktan HTTP pristup internim folderima. Uključite prikaz skrivenih fajlova pri upload-u. Ako hosting ne dozvoljava direktivu `Options`, uklonite samo liniju `Options -Indexes` i isključite listing u panelu hostinga.
- Nginx: prosledite `.php` fajlove PHP-FPM-u, postavite `index index.php;` i blokirajte interne foldere. Primer pravila u odgovarajućem `server` bloku (prilagodite prefiks ako hosting nije u `/cenovnik/`):

```nginx
location ~ ^/cenovnik/(data|includes|references|tests)/ { deny all; }
location ~ /\. { deny all; }
```

- PHP razvojni server ne primenjuje `.htaccess`; služi za lokalnu proveru. Za produkciju koristite hosting.
- Aktivirajte SSL u hosting panelu. Uključite preusmerenje sa HTTP na HTTPS kroz panel, nakon što sertifikat proradi. Koristite HTTPS adresu u QR kodu.
- Dovoljna su čitalačka prava za PHP nad podacima. Aplikaciji nisu potrebne dozvole za pisanje. Za standardni Linux hosting obično se koriste 755 za foldere i 644 za fajlove.
- Folderi `references` i `tests`, kao i `.md` uputstva, mogu ostati u vašoj lokalnoj kopiji; nisu potrebni na javnom hostingu. Napravite rezervnu kopiju `data` foldera pre većih izmena.
- Relativne putanje omogućavaju rad u korenu domena ili potfolderu. Nema spoljašnjih fontova, CDN poziva ili analitike tokom korišćenja.

## Ponašanje

Svako otvaranje početne adrese prikazuje izbor jezika. Jezik se nalazi u URL-u (`menu.php?lang=hu`), pa se može sačuvati ili podeliti. Nepoznat jezik vraća srpski. Pretraga ne razlikuje velika/mala slova i uobičajene dijakritike. Filter hrane/pića i pretraga rade zajedno; prazan upit vraća sve stavke odabranog filtera. Linkovi telefona rade na mobilnim uređajima. Stranice imaju stilove za štampu i podršku za tastaturu i smanjeno animiranje.
