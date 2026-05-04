# FARMBEAST – Modularni baterijski sistem

Ta repozitorij vsebuje konceptno dokumentacijo za nadgradnjo baterijskega sistema mobilnega kmetijskega robota FARMBEAST.

Glavni dokument opisuje prehod iz trenutnega enotnega LiFePO4 baterijskega sklopa na modularen Li-ion sistem, sestavljen iz več samostojnih 7S1P modulov z lastnim BMS.

## Vsebina

- izhodiščno stanje obstoječe baterije,
- predlagana 7S1P 20700 Li-ion arhitektura,
- osnovni napetostni, energijski in tokovni izračuni,
- zahteve za BMS,
- hot-swap zahteve,
- osnovna strategija polnjenja,
- naslednji koraki za električno, termično, mehansko in integracijsko preverjanje.

## Predlagana arhitektura

Predlagani sistem temelji na 6 ali 7 vzporedno vezanih baterijskih modulih. Vsak modul ima nazivno napetost približno 25.2 V, maksimalno napetost 29.4 V in energijo približno 75.6 Wh.

Pri 6 modulih sistem doseže približno 453.6 Wh, pri 7 modulih pa približno 529.2 Wh.

## Glavni cilj

Cilj projekta je izboljšati modularnost, servisiranje, diagnostiko, možnost menjave modulov in prihodnjo nadgradljivost baterijskega sistema robota FARMBEAST.

## Datoteke

- `FARMBEAST_PSF_koncept.md` – glavni tehnični koncept v Markdown obliki.
- `README.md` – kratek opis projekta in strukture.

## Status

Dokumentacija predstavlja začetni tehnični koncept. Pred izvedbo je treba dodatno preveriti tokovne omejitve, termično obnašanje, hot-swap zaščito, izbiro BMS, mehansko zasnovo modulov in način polnjenja.