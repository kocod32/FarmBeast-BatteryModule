# FARMBEAST – Koncept modularnega baterijskega sistema

Dokument opisuje osnovni koncept nadgradnje baterijskega sistema mobilnega kmetijskega robota FARMBEAST.

Trenutni baterijski sistem uporablja LiFePO4 baterijski paket. Predlagana rešitev je modularen sistem iz več manjših Li-ion modulov, vezanih vzporedno na skupni DC vod. Vsak modul je samostojna 7S1P enota z lastnim BMS-om, varovalko in izhodno zaščito.

---

## 1. Namen

Namen koncepta je preveriti, ali bi lahko trenutni LiFePO4 baterijski paket nadomestili z modularnim Li-ion sistemom.

Glavni cilji nadgradnje:

- lažja menjava baterijskih modulov,
- možnost uporabe različnega števila modulov,
- boljša servisabilnost,
- boljša diagnostika,
- možnost hot-swap priklopa,
- lažja prihodnja nadgradnja sistema.

---

## 2. Trenutni baterijski paket

Trenutni baterijski paket ima približno naslednje podatke:

| Parameter | Vrednost |
|---|---:|
| Kemija | LiFePO4 |
| Vezava celic | 8S1P |
| Nazivna napetost | 26.4 V |
| Maksimalna napetost | 28.3 V |
| Minimalna napetost | približno 18.4–22.9 V |
| Originalna kapaciteta | 12.8 Ah |
| Izmerjena kapaciteta | približno 12.1–12.6 Ah |
| Maksimalni tok | 60 A |



Energija enega originalnega paketa:

```text
E = U × Q
E = 26.4 V × 12.8 Ah
E = 337.92 Wh
```

Glede na izmerjeno kapaciteto je realna energija enega paketa približno:

```text
26.4 V × 12.1 Ah = 319.44 Wh
26.4 V × 12.6 Ah = 332.64 Wh
```

Za primerjavo lahko zato upoštevamo, da ima trenutni baterijski paket približno:

```text
320–338 Wh
```

---

## 3. Predlagan nov modul

Predlagan nov baterijski modul temelji na 20700 Li-ion celicah.

| Parameter | Vrednost |
|---|---:|
| Kemija | Li-ion |
| Tip celic | 20700 |
| Vezava | 7S1P |
| Kapaciteta | 3.0 Ah |
| Nazivna napetost | 25.2 V |
| Maksimalna napetost | 29.4 V |
| Minimalna napetost | približno 21.0 V |
| Energija | 75.6 Wh |

Napetostni izračun:

```text
U_nom = 7 × 3.6 V = 25.2 V
U_max = 7 × 4.2 V = 29.4 V
U_min = 7 × 3.0 V = 21.0 V
```

Energija enega modula:

```text
E = 25.2 V × 3.0 Ah
E = 75.6 Wh
```

Konfiguracija 7S Li-ion je napetostno blizu trenutnemu sistemu. Pred izvedbo je treba vseeno preveriti, ali robot in njegova elektronika trajno preneseta maksimalno napetost 29.4 V.

---

## 4. Predlagan modularni sistem

Predlagani sistem uporablja 6 ali 7 enakih 7S1P modulov v paralelni vezavi.

Pri paralelni vezavi:

- napetost sistema ostane približno enaka napetosti enega modula,
- kapaciteta se sešteva,
- energija se sešteva,
- tokovna obremenitev se porazdeli med module.

| Sistem | Kapaciteta | Energija |
|---|---:|---:|
| 1 modul | 3 Ah | 75.6 Wh |
| 6 modulov | 18 Ah | 453.6 Wh |
| 7 modulov | 21 Ah | 529.2 Wh |

Primerjava s trenutnim paketom:

| Sistem | Energija |
|---|---:|
| Trenutni LiFePO4 paket | približno 320–338 Wh |
| Nov sistem, 6 modulov | 453.6 Wh |
| Nov sistem, 7 modulov | 529.2 Wh |


---

## 5. Tokovna analiza

Trenutni baterijski paket omogoča maksimalni tok približno 60 A. Novi modularni sistem mora zato kot celota omogočati približno enak maksimalni tok.

Pri paralelni vezavi se skupni tok porazdeli med module. Ocenjeni tok na posamezen modul je:

```text
I_modul = I_skupni / število_modulov
```

Za skupni tok 60 A dobimo:

| Število modulov | Tok na en modul |
|---:|---:|
| 3 | 20.0 A |
| 6 | 10.0 A |
| 7 | 8.6 A |

Iz tega sledi:

- 1 ali 2 modula nista dovolj za polno obremenitev,
- 3 moduli so spodnja meja, če en modul varno prenese približno 20 A,
- 6 ali 7 modulov je primernejša konfiguracija, ker je tokovna obremenitev posameznega modula precej nižja.

Zato mora biti en modul dimenzioniran vsaj za približno 20 A, priporočljivo pa z dodatno rezervo. Za BMS je zato smiselna izbira vsaj 20 A kontinuirno oziroma približno 30 A z rezervo.

---

## 6. BMS in zaščita modula

Vsak modul mora imeti svoj BMS za 7S Li-ion baterijo.

BMS mora omogočati:

- zaščito pred prenapolnitvijo,
- zaščito pred podpraznitvijo,
- zaščito pred previsokim tokom,
- zaščito pred kratkim stikom,
- zaščito pred previsoko temperaturo,
- balansiranje celic.

Priporočena tokovna zmogljivost BMS-a:

```text
minimalno: 20 A kontinuirno
priporočljivo: 30 A kontinuirno
```

Za diagnostiko je smiselno uporabiti BMS s komunikacijo, na primer UART, CAN ali RS485.

---

## 7. Hot-swap

Hot-swap pomeni, da bi lahko modul priklopili ali odklopili med delovanjem sistema.

Samo BMS za to ni dovolj. Pri vzporedni vezavi baterijskih modulov lahko nastanejo:

- izenačevalni tokovi med moduli,
- povratni tok v modul,
- visok začetni tok,
- obremenitev konektorjev.

Zato mora imeti vsak modul dodatno izhodno zaščito.

Osnovna struktura modula:

```text
celice → BMS → varovalka → hot-swap / precharge stopnja → izhod modula
```

---

## 8. Polnjenje

Ker je novi sistem zasnovan kot 7S Li-ion, mora biti polnilec prilagojen tej kemiji.

Osnovne zahteve:

```text
način polnjenja: CC/CV
končna napetost: 29.4 V
```

Možni načini polnjenja:

- polnjenje posameznih modulov,
- skupno polnjenje modulov na DC vodu,
- servisna polnilna postaja.

---

## 9. Osnovna blokovna zasnova

Vsak modul vsebuje:

```text
20700 Li-ion celice
BMS
varovalko
hot-swap / precharge zaščito
močnostni konektor
```

Celoten sistem vsebuje:

```text
več modulov v paraleli
skupni DC vod
glavni kontaktor ali DC odklopnik
tokovni senzor
polnilni priključek
diagnostični vmesnik
```

Poenostavljena struktura:

```text
modul 1 ┐
modul 2 │
modul 3 ├── skupni DC vod ── zaščita ── FARMBEAST robot
...     │
modul 7 ┘
```

---

## 10. Kaj je treba še preveriti

Pred izvedbo je treba preveriti:

- ali robot dovoljuje maksimalno napetost 29.4 V,
- katere 20700 celice so primerne,
- kakšen tok lahko varno odda en modul,
- kateri BMS je primeren,
- kako rešiti hot-swap,
- kako dimenzionirati vodnike, konektorje in varovalke,
- kako rešiti polnjenje,
- kako mehansko vgraditi module,
- kako spremljati napetost, tok, temperaturo in napake.