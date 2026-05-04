# FARMBEAST – Modularni baterijski sistem

Repozitorij vsebuje osnovno dokumentacijo za koncept modularne nadgradnje baterijskega sistema mobilnega kmetijskega robota FARMBEAST.

Koncept obravnava prehod iz trenutnega enotnega LiFePO4 baterijskega sklopa na sistem več manjših Li-ion modulov, vezanih vzporedno. Vsak modul je zasnovan kot samostojna 7S1P enota z lastnim BMS.

## Vsebina

V repozitoriju so zbrani:

- osnovni opis predlagane arhitekture,
- napetostni, energijski in tokovni izračuni,
- zahteve za BMS,
- osnovne zahteve za hot-swap,
- predlog polnjenja,
- blokovni diagram sistema.

## Osnovna zasnova

Predlagani sistem temelji na 6 ali 7 vzporedno vezanih 7S1P Li-ion modulih iz 20700 celic.

En modul ima približno:

- 25.2 V nazivno napetost,
- 29.4 V maksimalno napetost,
- 3.0 Ah kapacitete,
- 75.6 Wh energije.

Pri 6 modulih sistem doseže približno 453.6 Wh, pri 7 modulih pa približno 529.2 Wh.

## Datoteke

- `FARMBEAST_battery_module_concept.md` – glavni tehnični koncept,
- `FARMBEAST_battery_system_7_modules.md` – blokovni diagram sistema,
- `README.md` – kratek opis repozitorija.

## Status

Dokumentacija predstavlja začetni tehnični koncept. Pred izvedbo so potrebni dodatni električni, termični, mehanski in varnostni izračuni ter izbira konkretnih komponent.