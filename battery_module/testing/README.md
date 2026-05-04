# FARMBEAST – Testni načrt za modularni baterijski sistem

Ta dokument opisuje teste, ki jih je treba izvesti pred dejansko uporabo modularnega baterijskega sistema na robotu FARMBEAST.

Testni načrt je razdeljen na tri ravni:

```text
1. test posamezne celice
2. test enega 7S1P modula
3. test sistema z več moduli v paraleli
```

Cilj testiranja je preveriti, ali je sistem napetostno, tokovno, termično in mehansko primeren za uporabo na robotu.

---

## 1. Varnost pred testiranjem

Pred začetkom testiranja:

- delati samo z nadzorom odgovorne osebe,
- uporabljati zaščitna očala in primeren delovni prostor,
- celice ne smejo biti poškodovane,
- moduli ne smejo biti kratkostično povezani,
- vsi kabli morajo biti pravilno dimenzionirani,
- varovalke morajo biti vgrajene pred večjimi tokovnimi testi,
- polnjenje in praznjenje mora biti ves čas nadzorovano,
- testov kratkega stika in zlorabnih testov se ne izvaja brez ustrezne opreme in nadzora.

Ta dokument ni certifikacijski postopek. Za končni produkt bi bilo treba upoštevati ustrezne standarde, na primer IEC 62133 in UN 38.3.

---

## 2. Testi posameznih celic

Namen: preveriti, ali so celice pred sestavo modula primerne in med seboj podobne.

| Test | Kaj preverimo | Kriterij |
|---|---|---|
| Vizualni pregled | poškodbe, udrtine, korozija, izolacija | celica brez poškodb |
| Meritev napetosti | začetna napetost celice | vse celice v podobnem območju |
| Meritev notranje upornosti | primerjava celic | izločiti očitno odstopajoče celice |
| Kapacitetni test | realna kapaciteta celice | blizu deklarirane vrednosti |
| Segrevanje pri praznjenju | temperatura pri obremenitvi | brez prekomernega segrevanja |

Celice za en modul naj bodo čim bolj podobne po napetosti, kapaciteti in notranji upornosti.

---

## 3. Test enega modula brez obremenitve

Namen: preveriti osnovno sestavo 7S1P modula.

| Test | Kaj preverimo |
|---|---|
| Napetost celotnega modula | ali je skupna napetost pravilna |
| Napetost posameznih členov | ali so vse celice pravilno priključene |
| Delovanje BMS-a | ali BMS pravilno zazna 7S konfiguracijo |
| Temperaturni senzor | ali BMS pravilno bere temperaturo |
| Komunikacija BMS-a | UART, CAN, RS485 ali Bluetooth |
| Izhod modula | pravilna polariteta MOD+ in MOD- |
| Varovalka | pravilna vgradnja in vrednost |

Pred nadaljnjim testiranjem mora biti polariteta izhoda jasno označena.

---

## 4. Test polnjenja enega modula

Namen: preveriti, ali se modul pravilno in varno polni.

Osnovni pogoji:

```text
kemija: Li-ion
konfiguracija: 7S
način polnjenja: CC/CV
končna napetost: 29.4 V
```

| Test | Kaj preverimo |
|---|---|
| Začetek polnjenja | ali polnilec pravilno začne polnjenje |
| CC faza | ali tok ostane omejen |
| CV faza | ali napetost ne preseže 29.4 V |
| Balansiranje | ali BMS uravnava napetosti členov |
| Temperatura | ali se modul prekomerno segreva |
| Zaključek polnjenja | ali polnilec pravilno zmanjša tok |

Kriterij: nobena celica ne sme preseči dovoljene maksimalne napetosti, modul pa se ne sme nevarno segrevati.

---

## 5. Test praznjenja enega modula

Namen: preveriti kapaciteto, napetostni padec in temperaturo modula.

Predlagani testi:

| Tok praznjenja | Namen |
|---:|---|
| 1 A | osnovni kapacitetni test |
| 5 A | primerjava z obstoječimi testiranji baterij |
| 10 A | delni delovni tok |
| 20 A | ciljna višja obremenitev modula |

Pri vsakem testu se beleži:

- tok,
- skupna napetost modula,
- napetost posameznih členov,
- temperatura,
- čas praznjenja,
- oddana kapaciteta,
- oddana energija.

Kriterij: modul mora ostati stabilen, BMS ne sme nepričakovano izklopiti modula, temperatura pa mora ostati v varnem območju.

---

## 6. Test BMS zaščit

Namen: preveriti, ali BMS pravilno zaščiti modul.

| Test | Kaj preverimo |
|---|---|
| Overvoltage protection | BMS izklopi polnjenje pri previsoki napetosti celice |
| Undervoltage protection | BMS izklopi praznjenje pri prenizki napetosti celice |
| Overcurrent protection | BMS izklopi pri previsokem toku |
| Temperature protection | BMS reagira pri previsoki temperaturi |
| Balancing | napetosti členov se pri polnjenju izenačujejo |
| Recovery | BMS se po napaki pravilno ponastavi |

Opomba: te teste je treba izvajati previdno in po navodilih proizvajalca BMS-a. Namen ni uničevanje modula, ampak preverjanje nastavitev in delovanja zaščit.

---

## 7. Test hot-swap / precharge stopnje

Namen: preveriti varno priklapljanje modula na DC vod.

| Test | Kaj preverimo |
|---|---|
| Priklop modula brez obremenitve | ali pride do iskrenja |
| Priklop modula na napolnjen DC vod | ali je začetni tok omejen |
| Razlika SOC med moduli | ali nastanejo previsoki izenačevalni tokovi |
| Odklop modula | ali sistem ostane stabilen |
| Povratni tok | ali tok ne teče nevarno nazaj v modul |

Pri tem testu je treba meriti tokovni sunek ob priklopu. Če je sunek previsok, hot-swap rešitev ni ustrezna.

---

## 8. Test treh modulov v paraleli

Namen: preveriti osnovno delovanje modularnega sistema pri več modulih.

Zakaj trije moduli:

```text
60 A / 3 moduli = 20 A na modul
```

To je spodnja smiselna meja, če je en modul dimenzioniran na približno 20 A.

| Test | Kaj preverimo |
|---|---|
| Enaka začetna napetost modulov | pred povezavo morajo biti napetosti podobne |
| Tokovna delitev | ali se tok približno enakomerno porazdeli |
| Segrevanje modulov | ali se kateri modul segreva bolj od drugih |
| Odklop enega modula | ali sistem ostane stabilen |
| Priklop dodatnega modula | ali hot-swap deluje |
| Praznjenje pri večjem toku | ali sistem zmore višjo obremenitev |

Če se tok med moduli ne deli dovolj enakomerno, je treba preveriti notranje upornosti, dolžine kablov, konektorje in zaščitne stopnje.

---

## 9. Test sistema s 6 ali 7 moduli

Namen: preveriti končno predlagano konfiguracijo.

| Test | Kaj preverimo |
|---|---|
| Skupna napetost sistema | napetost DC vodila |
| Skupna kapaciteta | približno 18 Ah pri 6 modulih ali 21 Ah pri 7 modulih |
| Skupna energija | približno 453.6 Wh pri 6 modulih ali 529.2 Wh pri 7 modulih |
| Tokovna delitev | tok po posameznih modulih |
| Temperatura | celice, BMS, konektorji, kabli |
| Delovanje diagnostike | napetosti, tokovi, temperature, napake |
| Stabilnost pri obremenitvi | padec napetosti in odziv BMS-ov |

Pri 60 A skupnega toka pričakujemo približno:

```text
6 modulov: 10 A na modul
7 modulov: 8.6 A na modul
```

---

## 10. Test na robotu

Namen: preveriti delovanje v realnih pogojih.

Testi:

- vklop robota,
- delovanje brez obremenitve,
- vožnja pri nizki obremenitvi,
- vožnja pri višji obremenitvi,
- meritev napetosti DC vodila,
- meritev toka,
- meritev temperature modulov,
- spremljanje napak BMS-a,
- test varnega izklopa,
- test menjave modula, če je hot-swap pripravljen.

Prvi testi na robotu naj bodo kratki in z omejenim tokom. Šele po stabilnem delovanju se lahko obremenitev povečuje.

---

## 11. Mehanski testi

Namen: preveriti, ali je modul primeren za uporabo na mobilnem robotu.

| Test | Kaj preverimo |
|---|---|
| Pritrditev modula | modul se ne premika |
| Vstavljanje in odstranjevanje | modul se lahko normalno menja |
| Konektorji | mehansko stabilni in pravilno vodeni |
| Vibracije | ni prekinitev ali poškodb |
| Zaščita kontaktov | ni možnosti kratkega stika |
| Ohišje | celice in elektronika so zaščitene |

---

## 12. Podatki, ki jih je treba beležiti

Za vsak test naj se beleži:

```text
datum testa
konfiguracija sistema
število modulov
napetost vsakega modula
napetost posameznih členov
tok sistema
tok posameznega modula
temperatura celic
temperatura BMS-a
čas testa
oddana kapaciteta
oddana energija
napake BMS-a
opombe
```

Priporočljivo je voditi meritve v tabeli, da se lahko primerjajo različni testi in različni moduli.

---

## 13. Kriteriji za uspešen prototip

Prototip je smiseln za nadaljnji razvoj, če:

- en modul stabilno deluje pri predvidenem toku,
- BMS pravilno zaščiti modul,
- polnjenje do 29.4 V deluje pravilno,
- moduli se pri vzporedni vezavi ne izenačujejo z nevarnimi tokovi,
- tok se med moduli deli dovolj enakomerno,
- konektorji in kabli se ne pregrevajo,
- sistem s 6 ali 7 moduli doseže pričakovano energijo,
- robot stabilno deluje z novim sistemom,
- mehanska menjava modulov je praktična in varna.

---

## 14. Predlagan vrstni red testiranja

```text
1. test posameznih celic
2. sestava enega modula
3. test polnjenja enega modula
4. test praznjenja enega modula
5. test BMS zaščit
6. test hot-swap/precharge stopnje
7. test treh modulov v paraleli
8. test šestih ali sedmih modulov
9. test na robotu
10. dolgoročni ciklični test
```

Tak vrstni red zmanjša tveganje, ker se najprej preveri en modul, šele nato pa več modulov in celoten sistem.