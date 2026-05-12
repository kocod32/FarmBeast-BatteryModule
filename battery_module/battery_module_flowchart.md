# FARMBEAST – Blokovni diagram baterijskega sistema

Diagram prikazuje osnovno arhitekturo modularnega baterijskega sistema. Namenjen je razumevanju sistema, ne pa natančni električni shemi.

## Pregled sistema

```mermaid
flowchart TB

    subgraph MODULES["Baterijski moduli"]
        direction TB
        M1["Modul 1<br/>7S1P Li-ion<br/>BMS + varovalka + hot-swap/precharge"]
        M2["Modul 2<br/>7S1P Li-ion<br/>BMS + varovalka + hot-swap/precharge"]
        M3["Modul 3<br/>7S1P Li-ion<br/>BMS + varovalka + hot-swap/precharge"]
        MN["Moduli 4–7<br/>enaka struktura"]
    end

    BUS["Skupni DC vod<br/>MOD+ / MOD-"]
    PROTECTION["Glavna zaščita<br/>DC kontaktor / odklopnik<br/>glavna DC varovalka"]
    MEASUREMENT["Meritve<br/>tokovni senzor / shunt<br/>napetost DC voda"]
    ROBOT["FARMBEAST robot<br/>21–29.4 V<br/>maks. 60 A"]

    CHARGER["Polnilec<br/>7S Li-ion CC/CV<br/>29.4 V"]
    DIAG["Diagnostika<br/>BMS podatki<br/>napetost / tok / temperatura / napake"]

    M1 --> BUS
    M2 --> BUS
    M3 --> BUS
    MN --> BUS

    BUS --> PROTECTION
    PROTECTION --> MEASUREMENT
    MEASUREMENT --> ROBOT

    CHARGER --> BUS

    M1 -. UART / CAN / RS485 .-> DIAG
    M2 -. UART / CAN / RS485 .-> DIAG
    M3 -. UART / CAN / RS485 .-> DIAG
    MN -. UART / CAN / RS485 .-> DIAG
```

## Notranja zgradba enega modula

```mermaid
flowchart LR

    CELLS["20700 Li-ion celice<br/>7S1P<br/>21700 kot možna alternativa"]
    BMS["BMS 7S<br/>balansiranje + zaščite"]
    FUSE["DC varovalka"]
    HOTSWAP["Hot-swap / precharge<br/>anti-spark<br/>zaščita pred povratnim tokom"]
    OUT["Izhod modula<br/>MOD+ / MOD-"]

    CELLS --> BMS
    BMS --> FUSE
    FUSE --> HOTSWAP
    HOTSWAP --> OUT

    BMS -. diagnostika .-> DIAG["UART / CAN / RS485"]
```