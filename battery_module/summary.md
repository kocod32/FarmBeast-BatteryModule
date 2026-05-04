# PSF - Koncept modularnega baterijskega sistema za FARMBEAST

## Tehnični koncept z osnovnimi izračuni

**Projekt:** Nadgradnja baterijskega sistema mobilnega kmetijskega robota FARMBEAST

**Predlagana arhitektura:**  
7S1P 20700 Li-ion moduli, 6 ali 7 modulov v paralelni vezavi, vsak modul z lastnim BMS.

---

## 1. Namen projekta

Cilj projekta je razviti nov modularni baterijski sistem za mobilnega kmetijskega robota FARMBEAST, ki bo nadomestil trenutni enotni baterijski modul LiFePO4 8S1P.

Nov sistem mora omogočati večjo modularnost, lažjo menjavo baterijskih modulov, možnost hot-swap, uporabo različnega števila modulov glede na nalogo, uporabo posameznega BMS na vsakem modulu, boljšo diagnostiko ter lažje servisiranje in transport.

Predvidena nova zasnova temelji na 20700 Li-ion celicah.

---

## 2. Izhodiščni podatki

Trenutna baterija robota:

- Kemija: LiFePO4
- Konfiguracija: 8S1P
- Nazivna napetost: 26.4 V
- Maksimalni tok: 60 A
- Kapaciteta: 12.8 Ah

Robot električno prenese območje od 21 V do 29.4 V, za normalno delovanje pa je zaželeno območje okoli 26 V.

### Energija trenutnega sistema

Formula:

```text
E = U × Q
E = 26.4 V × 12.8 Ah
E = 337.92 Wh
```

Trenutni baterijski sistem ima približno **337.92 Wh** energije.

---

## 3. Predlagana nova arhitektura

Osnovna ideja je, da novi sistem sestavlja več samostojnih baterijskih modulov, ki so napetostno enaki, med seboj vezani v paralelo, vsak z lastnim BMS in zmožni priklopa ali odklopa kot samostojna enota.

Ker želimo uporabiti 20700 Li-ion celice, je najbolj smiselna konfiguracija enega modula **7S1P Li-ion**.

To pomeni:

- 7 celic v seriji znotraj enega modula
- 1 paralelna veja na modul

---

## 4. Napetostni izračuni modula

Za tipično Li-ion celico velja:

- Nazivna napetost celice: 3.6 V
- Maksimalna napetost celice: 4.2 V
- Minimalna uporabna napetost celice: približno 3.0 V

Za 7S modul dobimo:

```text
U_nom = 7 × 3.6 V = 25.2 V
U_max = 7 × 4.2 V = 29.4 V
U_min = 7 × 3.0 V = 21.0 V
```

To se zelo dobro ujema z dovoljeno napetostjo robota od **21 V do 29.4 V**, zato je **7S Li-ion napetostno ustrezna izbira**.

---

## 5. Predlagano število modulov

Predvideno število modulov je **6 ali 7**.

Vsi moduli bodo priključeni vzporedno na skupni DC vod.

To pomeni:

- napetost sistema ostane približno enaka napetosti enega modula,
- kapaciteta se povečuje s številom modulov,
- maksimalni tok sistema se povečuje s številom modulov.

---

## 6. Predlog kapacitete enega modula

Za 20700 celice je realen delovni predlog kapaciteta celice **3.0 Ah** in nazivna napetost modula **25.2 V**.

Energija enega modula:

```text
E_modul = U_nom × Q_modul
E_modul = 25.2 V × 3.0 Ah
E_modul = 75.6 Wh
```

En modul vsebuje približno **75.6 Wh** energije, kar je uporabna in dovolj kompaktna vrednost.

---

## 7. Energijski izračuni sistema

Za modul 7S1P, 3.0 Ah, 25.2 V dobimo naslednje vrednosti:

| Konfiguracija | Napetost | Kapaciteta | Energija |
|---|---:|---:|---:|
| 1 modul | 25.2 V | 3.0 Ah | 75.6 Wh |
| 6 modulov | 25.2 V | 18 Ah | 453.6 Wh |
| 7 modulov | 25.2 V | 21 Ah | 529.2 Wh |

Primerjava s trenutnim sistemom pokaže, da ima novi sistem:

- pri 6 modulih približno **34 % več energije**,
- pri 7 modulih približno **57 % več energije**,

v primerjavi s trenutno baterijo, ki ima **337.92 Wh**.

---

## 8. Tokovna analiza

Zahtevani maksimalni tok sistema je **60 A**.

Če se tok med moduli deli približno enakomerno, velja:

```text
I_modul = I_total / n
```

| Število modulov | Tok na modul pri 60 A sistema |
|---:|---:|
| 1 | 60 A |
| 2 | 30 A |
| 3 | 20 A |
| 4 | 15 A |
| 5 | 12 A |
| 6 | 10 A |
| 7 | 8.6 A |

Ključna ugotovitev je, da bo sistem napetostno lahko deloval z 1 do 7 moduli, vendar polna moč pri 60 A ni realna z enim samim 7S1P modulom.

Če ciljamo, da en modul varno oddaja približno **20 A**, so za polno 60 A delovanje potrebni najmanj **3 moduli**.

Praktično to pomeni:

- 1 modul: testni režim ali nizka obremenitev,
- 2 modula: omejeno delovanje,
- 3 moduli ali več: polno delovanje,
- 6–7 modulov: najbolj ugoden delovni režim z najmanjšo obremenitvijo posameznega modula.

---

## 9. Zahtevane ciljne vrednosti modula

Predlagane ciljne vrednosti za en modul:

| Parameter | Vrednost |
|---|---:|
| Konfiguracija | 7S1P |
| Kemija | Li-ion 20700 |
| Nazivna napetost | 25.2 V |
| Maksimalna napetost | 29.4 V |
| Kapaciteta | 3.0 Ah |
| Energija | 75.6 Wh |
| Priporočeni trajni tok modula | vsaj 20 A |
| Kratkotrajni tok modula | 25–30 A |

Vsak modul mora biti opremljen z lastnim BMS in zaščito pred:

- prenapolnitvijo,
- podpraznitvijo,
- kratkim stikom,
- previsokim tokom,
- previsoko temperaturo.

---

## 10. Izbira BMS

Vsak modul mora imeti svoj BMS za **7S Li-ion**.

Zahteve za BMS:

- balansiranje celic,
- zaščita pred overcharge,
- zaščita pred undervoltage,
- zaščita pred overcurrent,
- zaščita pred short-circuit,
- zaščita pred overtemperature.

Tokovna zmogljivost BMS naj bo najmanj **20 A kontinuirno**, priporočljivo pa **30 A** zaradi rezerve.

Po možnosti naj ima BMS komunikacijo preko:

- UART,
- CAN,
- RS485.

Zaželena osnovna diagnostika:

- napetosti posameznih členov,
- temperatura,
- tok,
- status napake.

---

## 11. Hot-swap zahteve

Ker želimo možnost menjave modulov med obratovanjem, BMS sam po sebi ni dovolj.

Pri paralelnem povezovanju več baterijskih modulov se pojavijo naslednje težave:

- povratni tok med moduli,
- izenačevalni tok pri različnem SOC,
- iskrenje pri priklopu,
- visok inrush current.

Zato mora sistem vsebovati:

- zaščito proti povratnemu toku,
- kontroliran priklop modula na DC bus,
- anti-spark ali precharge rešitev,
- ustrezen močnostni konektor,
- jasno določene pogoje za priklop modula.

Tehnično priporočilo je, da vsak modul vsebuje:

- BMS,
- varovalko,
- izhodno zaščitno stopnjo za hot-swap.

---

## 12. Polnjenje sistema

Ker bo novi sistem osnovan na 7S Li-ion, mora biti nov polnilni sistem prilagojen tej kemiji.

Končna napetost polnjenja mora biti:

```text
U_charge = 29.4 V
```

Način polnjenja mora biti:

```text
CC/CV
```

Določiti bo treba tudi strategijo polnjenja:

- posamezno polnjenje modulov,
- skupno polnjenje vseh modulov,
- servisna polnilna postaja.

---

## 13. Kaj je treba izračunati in preveriti v naslednjem koraku

### Električni izračuni

Preveriti je treba:

- energijo posameznega modula,
- energijo sistema za 6 in 7 modulov,
- tok na modul za 1–7 modulov,
- minimalno število modulov za polno delovanje,
- predviden čas delovanja.

### Termični izračuni

Preveriti je treba:

- segrevanje celic pri 10 A, 15 A in 20 A,
- segrevanje povezav in vodnikov,
- termične rezerve znotraj modula.

### Mehanski del

Preveriti je treba:

- ohišje modula,
- pritrditev modula,
- konektor,
- zaščito pred vibracijami,
- servisni dostop,
- zaščito kontaktov.

### Integracijski del

Preveriti je treba:

- način priklopa modula na skupni vod,
- zaznavanje prisotnosti modula,
- preprečitev izpada ob menjavi,
- merjenje toka in napetosti na glavnem vodu.

---

## 14. Povzetek koncepta

Predlagani novi baterijski sistem za FARMBEAST temelji na več enakih modularnih baterijskih enotah iz 20700 Li-ion celic.

Vsak modul je:

- 7S1P,
- nazivno 25.2 V,
- maksimalno 29.4 V,
- kapacitete približno 3 Ah,
- energije približno 75.6 Wh,
- opremljen z lastnim BMS.

Sistem vsebuje 6 ali 7 modulov, vse module vezane v paralelo, možnost priklopa različnega števila modulov, hot-swap funkcionalnost ter večjo skupno energijo od trenutnega sistema.

Pri 6 modulih sistem doseže približno:

```text
18 Ah
453.6 Wh
```

Pri 7 modulih sistem doseže približno:

```text
21 Ah
529.2 Wh
```

Glavna projektna omejitev je, da en sam modul napetostno sicer ustreza sistemu, tokovno pa ne more nadomestiti celotnega sistema pri 60 A.

Zato je treba določiti minimalno število modulov za polno obremenitev. Realna delovna meja je približno **3 moduli ali več**.

---

## 15. Kratek zaključek za mentorja

Predlagana rešitev predstavlja prehod iz enotnega baterijskega sklopa na modularen sistem, ki omogoča bolj fleksibilno uporabo robota, lažjo menjavo baterij, boljšo diagnostiko in lažjo prihodnjo nadgradnjo.

Z uporabo 7S1P 20700 Li-ion modulov se doseže napetostna kompatibilnost z obstoječim robotom, pri čemer paralelna vezava več modulov omogoča povečanje kapacitete in tokovne zmogljivosti sistema.