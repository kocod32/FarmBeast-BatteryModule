# FARMBEAST – Blokovni diagram baterijskega sistema

Diagram prikazuje osnovno arhitekturo modularnega baterijskega sistema. Namenjen je razumevanju povezav med baterijskimi moduli, skupnim DC vodom, zaščito, polnjenjem, diagnostiko in robotom.

To ni natančna električna shema.

## Pregled sistema

```mermaid
flowchart TB

    subgraph MODULES["Baterijski moduli"]
        direction TB

        M1["Modul 1<br/>7S1P Li-ion<br/>BMS + varovalka + hot-swap/precharge"]
        M2["Modul 2<br/>7S1P Li-ion<br/>BMS + varovalka + hot-swap/precharge"]
        M3["Modul 3<br/>7S1P Li-ion<br/>BMS + varovalka + hot-swap/precharge"]
        M7["Moduli 4–7<br/>enaka struktura"]
    end

    BUS["Skupni DC vod<br/>MOD+ / MOD-"]

    PROTECTION["Glavna zaščita sistema<br/>DC kontaktor / odklopnik<br/>glavna DC varovalka"]

    MEASUREMENT["Meritve sistema<br/>tokovni senzor / shunt<br/>napetost DC voda"]

    ROBOT["FARMBEAST robot<br/>21–29.4 V<br/>maks. 60 A"]

    CHARGER["Polnilec<br/>7S Li-ion CC/CV<br/>29.4 V"]

    DIAG["Diagnostika<br/>BMS podatki<br/>napetost / tok / temperatura / napake"]

    M1 --> BUS
    M2 --> BUS
    M3 --> BUS
    M7 --> BUS

    BUS --> PROTECTION
    PROTECTION --> MEASUREMENT
    MEASUREMENT --> ROBOT

    CHARGER --> BUS

    M1 -. UART / CAN / RS485 .-> DIAG
    M2 -. UART / CAN / RS485 .-> DIAG
    M3 -. UART / CAN / RS485 .-> DIAG
    M7 -. UART / CAN / RS485 .-> DIAG

```

## Notranja zgradba enega modula

```mermaid
flowchart LR

    CELLS["20700 / 21700 Li-ion celice<br/>7S1P"]
    BMS["BMS 7S<br/>20–30 A<br/>balansiranje + zaščite"]
    FUSE["DC varovalka<br/>20–30 A"]
    HOTSWAP["Hot-swap / precharge zaščita<br/>anti-spark<br/>zaščita pred povratnim tokom"]
    OUT["Izhod modula<br/>MOD+ / MOD-"]

    CELLS --> BMS
    BMS --> FUSE
    FUSE --> HOTSWAP
    HOTSWAP --> OUT

    BMS -. diagnostika .-> DIAG["UART / CAN / RS485"]

```