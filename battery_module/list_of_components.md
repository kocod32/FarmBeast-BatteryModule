# Seznam komponent

Ta dokument vsebuje osnovni seznam komponent za prototip modularnega baterijskega sistema FARMBEAST.


## 1. En modul 7S1P

| Komponenta | Opis | Količina | Status |
|---|---|---:|---|
| Li-ion celice | 20700 ali 21700, vsaj 3 Ah, od preverjenega dobavitelja, npr. NKON ali Sportlampa | 7 kos | obvezno |
| BMS 7S | JBD ali Daly, vsaj 20 A kontinuirno, priporočljivo UART/Bluetooth za diagnostiko | 1 kos | obvezno |
| DC varovalka + nosilec | 20–30 A, na izhodu modula | 1 kos | obvezno |
| Nickel strip | dovolj debel za približno 20 A; celic se ne spajka, ampak točkovno vari | po potrebi | obvezno |
| Izolacijski material | fishpaper, Kapton trak, termo skrčka | po potrebi | obvezno |
| Močnostni konektor | XT90 anti-spark za prototip, industrijski konektor za končno verzijo | 1 par | obvezno |
| Hot-swap / precharge stopnja | omejitev začetnega toka; MOSFET ali ideal-diode rešitev | 1 kos | obvezno |
| Ohišje modula | 3D print za prototip, aluminij ali bolj robustna izvedba za končno verzijo | 1 kos | priporočljivo |

Opomba: temperaturni senzor in balansirni kabel nista posebej navedena, ker sta običajno del BMS kompleta.

## 2. Sistem s 6–7 moduli

| Komponenta | Opis | Količina | Status |
|---|---|---:|---|
| DC kontaktor ali odklopnik | vsaj 60 A, nujno DC-rated; AC kontaktor ni primeren za DC lok | 1 kos | obvezno |
| Glavna varovalka sistema | DC varovalka, nad 60 A | 1 kos | obvezno |
| Skupni DC vod | bakrena zbiralka ali pravilno dimenzionirani kabli za 60 A | 1 komplet | obvezno |
| 29.4 V polnilec | 7S Li-ion, CC/CV; ne LiFePO4 polnilec | 1 kos | obvezno |
| Tokovni senzor / shunt | vsaj 60 A, za meritev skupnega toka | 1 kos | priporočljivo |
| Diagnostični krmilnik | Arduino ali ESP32 za branje BMS podatkov; za prototip dovolj | 1 kos | priporočljivo |

Napetostni senzor ni posebej ločen, ker se meritev napetosti lahko vključi v diagnostični del sistema.
