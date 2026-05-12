# FARMBEAST – Modularni baterijski sistem

Repozitorij vsebuje osnovno dokumentacijo za koncept modularne nadgradnje baterijskega sistema robota FARMBEAST.

Ideja je zamenjava trenutnega LiFePO4 baterijskega paketa z modularnim Li-ion sistemom. Sistem je sestavljen iz več 7S1P modulov, vezanih vzporedno na skupni DC vod. Vsak modul ima svoj BMS, varovalko in hot-swap / precharge zaščito.

## Vsebina

- [TODO](battery_module/TODO.md)
- [Tehnični koncept](battery_module/battery_module_concept.md)
- [Blokovni diagram](battery_module/battery_module_flowchart.md)
- [Seznam komponent](battery_module/list_of_components.md)
- [Testiranje](battery_module/testing/README.md)
- [Trenutna baterija](battery_module/current_battery_module/)

## Osnovna zasnova

Osnovni izračun je narejen za 20700 Li-ion celice s kapaciteto približno 3.0 Ah. Kot alternativo je mogoče preveriti tudi 21700 celice.

En 7S1P modul ima približno:

- 25.2 V nazivno napetost,
- 29.4 V maksimalno napetost,
- 21.0 V minimalno uporabno napetost,
- 3.0 Ah kapacitete,
- 75.6 Wh energije.

Predlagana konfiguracija je 6 ali 7 modulov v paraleli:

- 6 modulov: približno 453.6 Wh,
- 7 modulov: približno 529.2 Wh.

Ciljni maksimalni tok celotnega sistema je približno 60 A, enako kot pri trenutnem baterijskem paketu.
