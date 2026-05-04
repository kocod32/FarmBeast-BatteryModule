# FARMBEAST – Seznam komponent za modularni baterijski sistem

Ta dokument je osnovni seznam komponent, ki bi jih bilo treba izbrati oziroma kupiti za izdelavo prototipa modularnega baterijskega sistema.

Koncept temelji na modulih **7S1P Li-ion**, vezanih vzporedno na skupni DC vod. Trenutno ima robot LiFePO4 baterijski paket približno **26.4 V, 12.8 Ah in 60 A maksimalnega toka**. Novi sistem mora zato napetostno ostati v podobnem območju in tokovno omogočati približno 60 A na ravni celotnega sistema.

---

## 1. Osnovna arhitektura

Predlagan sistem:

```text
7S1P modul 1 ┐
7S1P modul 2 │
7S1P modul 3 ├── skupni DC vod ── kontaktor / zaščita ── FARMBEAST robot
...          │
7S1P modul 7 ┘
```

Vsak modul vsebuje:

```text
Li-ion celice
BMS
varovalko
hot-swap / precharge zaščito
močnostni konektor
ohišje
```

---

## 2. Komponente za en modul

| Komponenta | Zahteva | Količina za 1 modul | Opomba |
|---|---|---:|---|
| Li-ion celice | 20700, približno 3.0 Ah, dovolj visok tok praznjenja | 7 | Celice morajo biti iz iste serije in od preverjenega dobavitelja. |
| BMS | 7S Li-ion, vsaj 20 A kontinuirno, priporočljivo 30 A | 1 | Zaželeno UART, CAN ali RS485 za diagnostiko. |
| Varovalka | DC varovalka, približno 20–30 A | 1 | Varovalka mora biti na izhodu modula. |
| Nosilec varovalke | Primeren za izbrano varovalko | 1 | Mora biti primeren za tok in napetost sistema. |
| Hot-swap / precharge stopnja | Omejitev začetnega toka in anti-spark | 1 | Lahko kot ločeno vezje ali MOSFET/ideal-diode rešitev. |
| Močnostni konektor | 30 A ali več, zanesljiv mehanski priklop | 1 par | Primer: Anderson, Amphenol, XT90 anti-spark za prototip. |
| Balansirni konektor / kabel | Za 7S BMS | 1 komplet | Običajno pride z BMS-om. |
| Temperaturni senzor | NTC za BMS | 1–2 | Vsaj ena meritev temperature na modul. |
| Ohišje modula | Mehanska zaščita celic in elektronike | 1 | 3D print, aluminij ali kombinacija. |
| Izolacijski materiali | fishpaper, Kapton trak, termo skrčka | po potrebi | Obvezno za varno sestavo paketa. |
| Nickel strip / povezave celic | Primerna debelina za tok | po potrebi | Ne spajkati neposredno na celice; uporabiti točkovno varjenje. |

---

## 3. Komponente za celoten sistem

| Komponenta | Zahteva | Količina | Opomba |
|---|---|---:|---|
| Skupni DC vod | + in - zbiralka | 1 komplet | Lahko bakrena zbiralka ali pravilno dimenzionirani kabli. |
| Glavni kontaktor ali DC odklopnik | Vsaj 60 A, primeren za DC | 1 | Mora odklopiti glavni napajalni vod. |
| Glavna varovalka sistema | Nad tokom sistema, primerna za DC | 1 | Za zaščito celotnega sistema. |
| Tokovni senzor ali shunt | 60 A ali več | 1 | Za merjenje glavnega toka. |
| Napetostni senzor | Meritev DC vodila | 1 | Za spremljanje napetosti sistema. |
| Diagnostični krmilnik | Branje BMS-ov in senzorjev | 1 | Mikrokrmilnik, Raspberry Pi, PLC ali obstoječa elektronika robota. |
| Polnilec | 7S Li-ion CC/CV, 29.4 V | 1 | Tok polnjenja določiti glede na izbrane celice in BMS. |
| Polnilni konektor | Primeren za tok polnjenja | 1 | Ločen od močnostnega izhoda, če je možno. |
| Glavni kabli | Primerni za 60 A | po potrebi | Dimenzionirati glede na tok, dolžino in padec napetosti. |
| Signalni kabli | Za BMS komunikacijo in senzorje | po potrebi | UART, CAN ali RS485. |
| Ohišje / nosilec sistema | Pritrditev modulov na robota | 1 | Mora omogočati hitro menjavo modulov. |

---

## 4. Količine za 6 ali 7 modulov

| Komponenta | Za 6 modulov | Za 7 modulov |
|---|---:|---:|
| Li-ion celice | 42 | 49 |
| BMS 7S | 6 | 7 |
| Varovalke modulov | 6 | 7 |
| Hot-swap / precharge stopnje | 6 | 7 |
| Močnostni konektorji modulov | 6 parov | 7 parov |
| Ohišja modulov | 6 | 7 |
| Temperaturni senzorji | 6–12 | 7–14 |

Priporočilo: pri nakupu celic kupiti nekaj dodatnih kosov za testiranje in rezervo.

Primer:

```text
7 modulov × 7 celic = 49 celic
priporočeno za prototip: približno 55 celic
```

---

## 5. Predlagani dobavitelji

### Celice

Za celice je smiselno uporabiti preverjene evropske dobavitelje, ker so ponaredki Li-ion celic pogosta težava.

Možni dobavitelji:

- NKON
- Sportlampa
- The Battery Shop EU
- drugi specializirani EU dobavitelji baterijskih celic

Opomba: če se izkaže, da je ponudba 20700 celic omejena, je smiselno preveriti tudi možnost uporabe 21700 celic. Te so danes pogostejše in lažje dostopne, vendar zahtevajo preverjanje mehanskih dimenzij modula.

### BMS

Možni tipi:

- JBD Smart BMS 7S Li-ion,
- Daly Smart BMS 7S Li-ion,
- drug 7S Li-ion BMS z znano dokumentacijo.

Minimalna zahteva:

```text
7S Li-ion
20 A kontinuirno
30 A priporočljivo
temperaturna zaščita
balansiranje
možnost diagnostike
```

Za prototip se lahko kupi en BMS za testiranje, preden se kupi vseh 6 ali 7 kosov.

### Konektorji, varovalke, kontaktorji, kabli

Možni dobavitelji:

- TME
- RS
- Mouser
- DigiKey
- Farnell
- lokalne elektro trgovine za kable, sponke in osnovni material

Za močnostne konektorje je bolje uporabiti preverjene industrijske konektorje. XT90 anti-spark je lahko uporaben za prototip, ni pa nujno najboljša izbira za končno izvedbo na robotu.

---

## 6. Kaj kupiti najprej

Za prvi praktični prototip ni treba takoj kupiti vseh komponent za 7 modulov.

Najprej je smiselno kupiti material za **en testni modul**:

| Komponenta | Količina |
|---|---:|
| Li-ion celice | 7 + nekaj rezervnih |
| 7S Li-ion BMS | 1 |
| Varovalka in nosilec | 1 |
| Močnostni konektor | 1 par |
| Material za povezavo celic | po potrebi |
| Izolacijski material | po potrebi |
| Ohišje prototipa | 1 |
| 29.4 V polnilec | 1 |
| Merilna oprema | po potrebi |

Po uspešnem testu enega modula se lahko nadaljuje na 3 module, ker so 3 moduli spodnja meja za približno 60 A, če je vsak modul dimenzioniran na 20 A.

Šele po testu 3 modulov je smiselno izdelati 6 ali 7 modulov.

---

## 7. Merilna in delovna oprema

Za sestavo in testiranje so potrebni:

- nastavljiv laboratorijski napajalnik,
- elektronsko breme ali primeren uporovni porabnik,
- multimeter,
- tokovne klešče ali tokovni senzor,
- termometer ali termalna kamera,
- točkovni varilnik za celice,
- izolacijski material,
- osnovno orodje za kable in konektorje,
- gasilni in varnostni ukrepi za delo z Li-ion baterijami.

Pri sestavljanju celic se ne sme spajkati direktno na celice. Za povezavo celic je treba uporabiti točkovno varjenje ali profesionalno pripravljene celice s priključki.

---

## 8. Kratek nakupni plan

Priporočen vrstni red:

```text
1. izbrati konkretno celico
2. kupiti celice za en modul
3. kupiti en 7S BMS
4. izdelati en testni modul
5. izvesti osnovne teste
6. izdelati tri module
7. testirati tokovno delitev in obnašanje na DC vodu
8. šele nato kupiti material za 6 ali 7 modulov
```

Tako se zmanjša tveganje, da bi kupili večje število neustreznih celic, BMS-ov ali konektorjev.