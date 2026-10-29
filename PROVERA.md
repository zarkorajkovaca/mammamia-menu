# Izveštaj provere — 27. oktobar 2026, korekcija dodataka

- „Džem” i „Orasi” sada su dve zasebne stavke u Dodacima, svaka po 80 RSD.
- Obe stavke imaju nazive na svih šest jezika. Zajednička stavka uklonjena je iz podataka i prikaza.
- `php tests/validate.php`: PASS — 95 stavki, 145 cenovnih varijanti, 13 kategorija i šest jezika, od toga 19 dodataka.
- PHP sintaksa izmenjenog validacionog fajla prolazi.
- Svih šest menija generisano je pravim PHP runtime-om. U svakom su provereni odvojeni nazivi i cene 80 RSD za oba dodatka, kao i ukupan broj stavki i cena.
- Poređenje sa prethodnim ZIP-om potvrđuje da su sve druge stavke, njihovi prevodi, cene i kategorije identični. Nezavisni pregled potvrđuje istu izmenu.

Prethodna verzija prošla je provere prikaza šest jezika na širinama 320–1440 px, pretrage, filtera, serverskog prikaza bez JavaScript-a i očitavanja generisanog QR koda. Ova korekcija menja podatke dodataka, dokumentaciju i postojeću validacionu proveru. Novi vizuelni test nije ponavljan za razdvajanje dva kratka naziva.

Provera QR-a u prethodnoj verziji obuhvatila je generisani SVG i pripremljeni download link; automatsko čuvanje preko pregledača u test okruženju bilo je otkazano. Hosting i stvarnu javnu adresu proverite nakon postavljanja. Ako adresa ostaje ista, postojeći QR nastavlja da radi.
