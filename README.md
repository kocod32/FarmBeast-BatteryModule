# FARMBEAST – Modularni baterijski sistem

Repozitorij vsebuje osnovno dokumentacijo za koncept modularne nadgradnje baterijskega sistema mobilnega kmetijskega robota FARMBEAST.

Koncept obravnava zamenjavo trenutnega LiFePO4 baterijskega paketa z modularnim Li-ion sistemom. Predlagana zasnova temelji na več manjših 7S1P Li-ion modulih, vezanih vzporedno na skupni DC vod. Vsak modul ima svoj BMS, varovalko in hot-swap / precharge zaščito.

## Vsebina repozitorija

- [Tehnični koncept](battery_module/battery_module_concept.md) – osnovni opis predlaganega modularnega baterijskega sistema.
- [Blokovni diagram](battery_module/battery_module_flowchart.md) – Mermaid diagram arhitekture sistema in enega modula.
- [Seznam komponent](battery_module/list_of_components.md) – osnovni seznam komponent za prototip.
- [Testiranje](battery_module/testing/README.md) – kratek pregled testov, ki jih je smiselno izvesti.
- [Trenutna baterija](battery_module/current_battery_module/) – podatki in meritve trenutnega baterijskega sistema.

## Osnovna zasnova

Predlagani sistem temelji na 6 ali 7 vzporedno vezanih 7S1P Li-ion modulih iz 20700 ali 21700 celic.

En modul ima približno:

- 25.2 V nazivno napetost,
- 29.4 V maksimalno napetost,
- 3.0 Ah kapacitete,
- 75.6 Wh energije.

Pri 6 modulih sistem doseže približno 453.6 Wh, pri 7 modulih pa približno 529.2 Wh.

## Status

Dokumentacija predstavlja začetni tehnični koncept. Pred izvedbo je treba izbrati konkretne komponente in izvesti dodatne električne, termične, mehanske in varnostne teste.