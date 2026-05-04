# FARMBEAST – Modularni baterijski sistem

Repozitorij vsebuje osnovno dokumentacijo za koncept modularne nadgradnje baterijskega sistema mobilnega kmetijskega robota FARMBEAST.

Koncept obravnava zamenjavo trenutnega LiFePO4 baterijskega paketa z modularnim Li-ion sistemom. Predlagana zasnova temelji na več manjših 7S1P Li-ion modulih, vezanih vzporedno na skupni DC vod. Vsak modul ima svoj BMS, varovalko in izhodno zaščito.

## Vsebina repozitorija

- `battery_module/battery_module_concept.md` – osnovni tehnični koncept modularnega baterijskega sistema.
- `battery_module/battery_module_flowchart.md` – blokovni Mermaid diagram sistema.
- `battery_module/list_of_components.md` – seznam potrebnih komponent za prototip.
- `battery_module/testing/README.md` – testni načrt za celice, module in celoten sistem.
- `battery_module/current_battery_module/` – podatki o trenutnem baterijskem sistemu.

## Osnovna zasnova

Predlagani sistem temelji na 6 ali 7 vzporedno vezanih 7S1P Li-ion modulih iz 20700 celic.

En modul ima približno:

- 25.2 V nazivno napetost,
- 29.4 V maksimalno napetost,
- 3.0 Ah kapacitete,
- 75.6 Wh energije.

Pri 6 modulih sistem doseže približno 453.6 Wh, pri 7 modulih pa približno 529.2 Wh.

## Status

Dokumentacija predstavlja začetni tehnični koncept. Pred izvedbo so potrebni dodatni električni, termični, mehanski in varnostni izračuni ter izbira konkretnih komponent.