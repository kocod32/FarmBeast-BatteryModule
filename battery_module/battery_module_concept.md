# FARMBEAST – Koncept modularnega baterijskega sistema

Dokument opisuje koncept nadgradnje baterijskega sistema mobilnega kmetijskega robota FARMBEAST.

Trenutni sistem uporablja enoten LiFePO4 baterijski sklop. Predlagana nadgradnja temelji na več manjših Li-ion modulih, vezanih vzporedno na skupni DC vod. Vsak modul je zasnovan kot samostojna 7S1P enota z lastnim BMS-om.

## 1. Namen

Namen koncepta je preveriti, ali je mogoče obstoječi baterijski sistem zamenjati z modularno zasnovo, ki omogoča:

- lažjo menjavo baterijskih modulov,
- uporabo različnega števila modulov glede na nalogo,
- boljšo servisabilnost,
- boljšo diagnostiko posameznih modulov,
- možnost hot-swap priklopa,
- lažjo prihodnjo nadgradnjo sistema.

Predlagana zasnova uporablja 20700 Li-ion celice.

## 2. Izhodiščno stanje

Trenutna baterija robota:

| Parameter | Vrednost |
|---|---:|
| Kemija | LiFePO4 |
| Konfiguracija | 8S1P |
| Nazivna napetost | 26.4 V |
| Kapaciteta | 12.8 Ah |
| Maksimalni tok | 60 A |

Robot električno deluje v območju približno od 21 V do 29.4 V. Za normalno delovanje je zaželeno območje okoli 26 V.

Energija trenutnega baterijskega sistema:

```text
E = U × Q
E = 26.4 V × 12.8 Ah
E = 337.92 Wh
```

Trenutni sistem ima približno **337.9 Wh** energije.

## 3. Predlagana arhitektura

Predlagani sistem je sestavljen iz več enakih baterijskih modulov.

Vsak modul je:

- 7S1P,
- sestavljen iz 20700 Li-ion celic,
- opremljen z lastnim BMS-om,
- priključen na skupni DC vod,
- zasnovan kot samostojna zamenljiva enota.

Predvideno število modulov je **6 ali 7**.

Moduli so vezani vzporedno. To pomeni, da napetost sistema ostane enaka napetosti enega modula, kapaciteta in razpoložljiv tok pa se povečujeta s številom priključenih modulov.

## 4. Napetostni izračun modula

Za tipično Li-ion celico predpostavimo:

| Parameter | Vrednost |
|---|---:|
| Nazivna napetost celice | 3.6 V |
| Maksimalna napetost celice | 4.2 V |
| Minimalna uporabna napetost celice | približno 3.0 V |

Za 7S modul:

```text
U_nom = 7 × 3.6 V = 25.2 V
U_max = 7 × 4.2 V = 29.4 V
U_min = 7 × 3.0 V = 21.0 V
```

Napetostni razpon 7S Li-ion modula je zato približno **21.0 V do 29.4 V**, kar se ujema z dovoljenim območjem robota.

Zato je konfiguracija **7S Li-ion** napetostno primerna za ta sistem.

## 5. Kapaciteta in energija enega modula

Za osnovni izračun je uporabljena 20700 Li-ion celica s kapaciteto **3.0 Ah**.

Ker je modul 7S1P, ima celoten modul enako kapaciteto kot ena celica, torej **3.0 Ah**.

Energija enega modula:

```text
E_modul = U_nom × Q_modul
E_modul = 25.2 V × 3.0 Ah
E_modul = 75.6 Wh
```

En modul ima približno **75.6 Wh** energije.

## 6. Energija celotnega sistema

| Konfiguracija | Napetost | Kapaciteta | Energija |
|---|---:|---:|---:|
| 1 modul | 25.2 V | 3.0 Ah | 75.6 Wh |
| 6 modulov | 25.2 V | 18.0 Ah | 453.6 Wh |
| 7 modulov | 25.2 V | 21.0 Ah | 529.2 Wh |

Primerjava s trenutnim sistemom:

| Sistem | Energija |
|---|---:|
| Trenutni LiFePO4 sistem | 337.9 Wh |
| Nov sistem, 6 modulov | 453.6 Wh |
| Nov sistem, 7 modulov | 529.2 Wh |

Pri 6 modulih ima sistem približno **34 % več energije** kot trenutna baterija.

Pri 7 modulih ima sistem približno **57 % več energije** kot trenutna baterija.

## 7. Tokovna analiza

Zahtevani maksimalni tok sistema je približno **60 A**.

Če predpostavimo približno enakomerno delitev toka med moduli, velja:

```text
I_modul = I_total / n
```

| Število modulov | Tok na modul pri 60 A sistema |
|---:|---:|
| 1 | 60.0 A |
| 2 | 30.0 A |
| 3 | 20.0 A |
| 4 | 15.0 A |
| 5 | 12.0 A |
| 6 | 10.0 A |
| 7 | 8.6 A |

Iz tega sledi, da en sam 7S1P modul ne more realno nadomestiti celotnega sistema pri 60 A.

Če je cilj, da en modul varno oddaja približno **20 A**, so za polno obremenitev potrebni najmanj **3 moduli**.

Praktična interpretacija:

| Število modulov | Predviden režim |
|---:|---|
| 1 modul | testno ali zelo omejeno delovanje |
| 2 modula | omejeno delovanje |
| 3 moduli | spodnja meja za polno tokovno obremenitev |
| 6–7 modulov | normalen delovni režim z manjšo obremenitvijo posameznega modula |

Najbolj smiseln delovni režim je zato uporaba **6 ali 7 modulov**, saj je takrat tokovna obremenitev posameznega modula precej manjša.

## 8. Ciljne vrednosti enega modula

| Parameter | Vrednost |
|---|---:|
| Konfiguracija | 7S1P |
| Kemija | Li-ion |
| Tip celic | 20700 |
| Nazivna napetost | 25.2 V |
| Maksimalna napetost | 29.4 V |
| Minimalna uporabna napetost | približno 21.0 V |
| Kapaciteta | 3.0 Ah |
| Energija | 75.6 Wh |
| Priporočeni trajni tok | vsaj 20 A |
| Kratkotrajni tok | 25–30 A |

Vsak modul mora imeti zaščito pred:

- prenapolnitvijo,
- podpraznitvijo,
- previsokim tokom,
- kratkim stikom,
- previsoko temperaturo.

## 9. BMS

Vsak modul potrebuje svoj BMS za **7S Li-ion** konfiguracijo.

Osnovne zahteve za BMS:

- balansiranje celic,
- zaščita pred prenapolnitvijo,
- zaščita pred podpraznitvijo,
- zaščita pred previsokim tokom,
- zaščita pred kratkim stikom,
- zaščita pred previsoko temperaturo.

Priporočena tokovna zmogljivost BMS-a:

```text
minimalno: 20 A kontinuirno
priporočljivo: 30 A kontinuirno
```

Za diagnostiko je smiselno uporabiti BMS s komunikacijo, na primer:

- UART,
- CAN,
- RS485.

Zaželene diagnostične informacije:

- napetosti posameznih členov,
- temperatura modula,
- tok modula,
- stanje napolnjenosti,
- status napake.

## 10. Hot-swap

Hot-swap pomeni možnost priklopa ali odklopa baterijskega modula med delovanjem sistema.

BMS sam po sebi za to ni dovolj.

Pri vzporednem povezovanju baterijskih modulov se lahko pojavijo:

- izenačevalni tokovi med moduli,
- povratni tok v modul,
- iskrenje ob priklopu,
- visok inrush current,
- obremenitev konektorjev,
- napetostni skoki na DC vodu.

Zato mora imeti vsak modul poleg BMS-a še ustrezno izhodno zaščitno stopnjo.

Priporočena sestava modula:

```text
celice → BMS → varovalka → hot-swap/precharge stopnja → izhod modula
```

Hot-swap stopnja mora omogočati:

- kontroliran priklop na DC vod,
- omejitev začetnega toka,
- anti-spark funkcijo,
- zaščito proti povratnemu toku,
- varno mehansko povezavo preko ustreznega konektorja.

## 11. Polnjenje

Ker je predlagani sistem osnovan na 7S Li-ion konfiguraciji, mora biti tudi polnilec prilagojen tej kemiji.

Končna napetost polnjenja:

```text
U_charge = 29.4 V
```

Način polnjenja:

```text
CC/CV
```

Možne strategije polnjenja:

- polnjenje posameznih modulov,
- skupno polnjenje vseh modulov na DC vodu,
- servisna polnilna postaja za module.

Najbolj varna in pregledna možnost je ločena servisna polnilna postaja, kjer se lahko posamezni moduli polnijo in preverjajo izven robota.

## 12. Blokovna zasnova sistema

Osnovna struktura sistema:

```text
7S1P modul 1 ┐
7S1P modul 2 ├── skupni DC vod ── kontaktor / zaščita ── FARMBEAST robot
7S1P modul 3 │
...          │
7S1P modul 7 ┘
```

Vsak modul vsebuje:

```text
20700 Li-ion celice
BMS
varovalko
hot-swap/precharge stopnjo
močnostni konektor
```

Skupni sistem vsebuje:

```text
DC vod
glavni kontaktor ali DC odklopnik
tokovni senzor ali shunt
napetostne meritve
polnilni priključek
diagnostični vmesnik
```

## 13. Nadaljnje preverjanje

Pred izvedbo je treba podrobneje preveriti več področij.

### Električni del

- izbira konkretnih 20700 celic,
- maksimalni dovoljeni tok celic,
- realna tokovna delitev med moduli,
- izbor BMS-a,
- izbor varovalk,
- dimenzioniranje vodnikov,
- dimenzioniranje konektorjev,
- precharge in hot-swap vezje,
- meritev toka in napetosti.

### Termični del

- segrevanje celic pri različnih tokovih,
- segrevanje vodnikov,
- segrevanje konektorjev,
- hlajenje modula,
- maksimalna temperatura v zaprtem ohišju.

### Mehanski del

- ohišje modula,
- pritrditev modula na robota,
- zaščita pred vibracijami,
- zaščita kontaktov,
- servisni dostop,
- enostavna menjava modula.

### Integracijski del

- zaznavanje prisotnosti modula,
- komunikacija z BMS-i,
- prikaz napak,
- stanje napolnjenosti sistema,
- obnašanje sistema pri odstranitvi ali dodajanju modula,
- varno polnjenje.

## 14. Povzetek

Predlagani baterijski sistem za FARMBEAST uporablja več enakih 7S1P Li-ion modulov iz 20700 celic.

En modul ima približno:

```text
25.2 V nazivno napetost
29.4 V maksimalno napetost
3.0 Ah kapacitete
75.6 Wh energije
```

Celoten sistem s 6 moduli doseže približno:

```text
18 Ah
453.6 Wh
```

Celoten sistem s 7 moduli doseže približno:

```text
21 Ah
529.2 Wh
```

Glavna prednost predlagane zasnove je modularnost. Sistem bi omogočal lažje servisiranje, boljšo diagnostiko in prilagoditev števila modulov glede na nalogo.

Glavna tehnična omejitev je tok. En sam modul ne more zagotoviti celotnega zahtevanega toka 60 A, zato je za polno delovanje potrebnih več modulov. Realna spodnja meja za polno obremenitev je približno 3 moduli, priporočena uporaba pa 6 ali 7 modulov.

## 15. Status koncepta

Dokument predstavlja začetni tehnični koncept.

Pred dejansko izvedbo je treba izbrati konkretne komponente in narediti podrobnejše električne, termične, mehanske in varnostne izračune.