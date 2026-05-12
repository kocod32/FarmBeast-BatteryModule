# FARMBEAST – Koncept modularnega baterijskega sistema

Dokument opisuje osnovni koncept nadgradnje baterijskega sistema mobilnega kmetijskega robota FARMBEAST.

Trenutni sistem uporablja LiFePO4 baterijski paket. Predlagana rešitev je modularen Li-ion sistem iz več manjših 7S1P modulov, vezanih vzporedno na skupni DC vod. Vsak modul ima svoj BMS, varovalko in hot-swap / precharge zaščito.

---

## 1. Trenutni baterijski paket

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

Energija enega paketa je približno:

```text
E = 26.4 V × 12.8 Ah = 337.92 Wh
```

Glede na izmerjeno kapaciteto je realna energija enega paketa približno **320–338 Wh**.

---

## 2. Predlagan nov modul

Osnovni izračun je narejen za **20700 Li-ion celice** s kapaciteto približno 3.0 Ah. Kot alternativo je mogoče preveriti tudi **21700 celice**, vendar je treba pri tem ponovno preveriti dimenzije, kapaciteto, tokovno zmogljivost in energijo modula.

| Parameter | Vrednost |
|---|---:|
| Kemija | Li-ion |
| Tip celic | 20700 |
| Vezava | 7S1P |
| Kapaciteta | 3.0 Ah |
| Nazivna napetost | 25.2 V |
| Maksimalna napetost | 29.4 V |
| Minimalna uporabna napetost | približno 21.0 V |
| Energija | 75.6 Wh |

Napetostni izračun:

```text
U_nom = 7 × 3.6 V = 25.2 V
U_max = 7 × 4.2 V = 29.4 V
U_min = 7 × 3.0 V = 21.0 V
```

Energija enega modula:

```text
E = 25.2 V × 3.0 Ah = 75.6 Wh
```

Konfiguracija 7S Li-ion je napetostno blizu trenutnemu sistemu. Pred izvedbo je treba preveriti, ali robot in njegova elektronika dovoljujeta celotno napetostno območje novega sistema, približno **21.0–29.4 V**.

---

## 3. Predlagan modularni sistem

Predlagani sistem uporablja **6 ali 7 enakih 7S1P modulov** v paralelni vezavi.

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

## 4. Tokovna analiza

Trenutni baterijski paket omogoča maksimalni tok približno **60 A**. Novi modularni sistem mora zato kot celota omogočati približno enak maksimalni tok.

Tokovna zmogljivost enega novega modula ni določena samo s konfiguracijo 7S1P, ampak predvsem z izbranimi komponentami:

- izbranimi celicami,
- BMS-om,
- varovalko,
- nickel stripom,
- vodniki,
- konektorji,
- hlajenjem in temperaturo modula.

Pri paralelni vezavi se skupni tok približno porazdeli med module:

```text
I_modul = I_skupni / število_modulov
```

Za skupni tok 60 A dobimo:

| Število modulov | Tok na en modul pri 60 A |
|---:|---:|
| 3 | 20.0 A |
| 6 | 10.0 A |
| 7 | 8.6 A |

To pomeni:

- če želimo delovanje s 6 ali 7 moduli, mora en modul varno oddajati približno 9–10 A,
- če želimo možnost delovanja s samo 3 moduli pri polni obremenitvi, mora en modul varno oddajati približno 20 A,
- 1 ali 2 modula nista predvidena za polno obremenitev 60 A.

Za prototip je zato smiselno izbrati celice in BMS z rezervo. BMS 20–30 A je primerna začetna izbira, vendar je dejanska tokovna zmogljivost modula odvisna od konkretnih komponent in testiranja.

---

## 5. BMS, zaščita in hot-swap

Vsak modul mora imeti svoj BMS za 7S Li-ion baterijo.

BMS mora omogočati osnovne zaščite:

- prenapolnitev,
- podpraznitev,
- previsok tok,
- kratek stik,
- previsoka temperatura,
- balansiranje celic.

Ker so moduli vezani vzporedno, samo BMS ni dovolj za varen priklop. Vsak modul mora imeti še izhodno zaščito za hot-swap oziroma precharge.

Osnovna struktura modula:

```text
celice → BMS → varovalka → hot-swap / precharge zaščita → izhod modula
```

Hot-swap / precharge zaščita mora zmanjšati iskrenje, omejiti začetni tok in preprečiti nevaren povratni tok med moduli.

---

## 6. Polnjenje

Ker je novi sistem 7S Li-ion, mora biti polnilec prilagojen Li-ion kemiji.

Osnovne zahteve:

```text
način polnjenja: CC/CV
končna napetost: 29.4 V
```

LiFePO4 polnilec ni primeren, ker ima drugačen napetostni profil.

Za začetni razvoj je najbolj pregledna rešitev servisna polnilna postaja, kjer se lahko posamezni moduli polnijo in preverjajo ločeno.

---

## 7. Osnovna blokovna zasnova

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
tokovni senzor ali shunt
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

## 8. Kaj je treba še preveriti

Pred izvedbo je treba preveriti:

- ali robot dovoljuje celotno napetostno območje novega sistema, približno 21.0–29.4 V,
- ali so za izvedbo primernejše 20700 ali 21700 celice,
- kakšen tok lahko varno odda en modul,
- kateri BMS je primeren,
- kako rešiti hot-swap / precharge zaščito,
- kako dimenzionirati vodnike, konektorje in varovalke,
- kako rešiti polnjenje,
- kako mehansko vgraditi module,
- kako spremljati napetost, tok, temperaturo in napake.