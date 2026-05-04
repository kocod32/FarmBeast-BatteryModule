# FARMBEAST – Blokovni diagram baterijskega sistema

Ta dokument vsebuje poenostavljen Mermaid diagram modularnega baterijskega sistema. Diagram je namenjen prikazu arhitekture sistema, ne pa natančni električni shemi.

## Pregled sistema

```mermaid
flowchart TB

    %% =========================
    %% FARMBEAST battery system
    %% System overview
    %% =========================

    subgraph MODULES["Baterijski moduli"]
        direction TB

        M1["Modul 1<br/>7S1P Li-ion<br/>25.2 V / 75.6 Wh"]
        M2["Modul 2<br/>7S1P Li-ion<br/>25.2 V / 75.6 Wh"]
        M3["Modul 3<br/>7S1P Li-ion<br/>25.2 V / 75.6 Wh"]
        M4["Modul 4<br/>7S1P Li-ion<br/>25.2 V / 75.6 Wh"]
        M5["Modul 5<br/>7S1P Li-ion<br/>25.2 V / 75.6 Wh"]
        M6["Modul 6<br/>7S1P Li-ion<br/>25.2 V / 75.6 Wh"]
        M7["Modul 7<br/>7S1P Li-ion<br/>25.2 V / 75.6 Wh"]
    end

    subgraph BUS["Skupni DC vod"]
        direction TB
        BUSP["+ DC BUS"]
        BUSM["- DC BUS"]
    end

    subgraph POWER["Močnostni izhod"]
        direction TB
        K["Glavni kontaktor<br/>DC odklopnik"]
        SHUNT["Tokovni senzor<br/>ali shunt"]
        ROBOT["FARMBEAST robot<br/>21–29.4 V<br/>maks. 60 A"]
    end

    CHG["Polnilec / servisna postaja<br/>7S Li-ion CC/CV<br/>29.4 V"]
    CTRL["Diagnostika / krmilnik<br/>napetosti členov<br/>tok<br/>temperatura<br/>napake"]

    M1 --> BUSP
    M2 --> BUSP
    M3 --> BUSP
    M4 --> BUSP
    M5 --> BUSP
    M6 --> BUSP
    M7 --> BUSP

    M1 --> BUSM
    M2 --> BUSM
    M3 --> BUSM
    M4 --> BUSM
    M5 --> BUSM
    M6 --> BUSM
    M7 --> BUSM

    BUSP --> K
    K --> ROBOT

    BUSM --> SHUNT
    SHUNT --> ROBOT

    CHG --> BUSP
    CHG --> BUSM

    M1 -. UART / CAN / RS485 .-> CTRL
    M2 -. UART / CAN / RS485 .-> CTRL
    M3 -. UART / CAN / RS485 .-> CTRL
    M4 -. UART / CAN / RS485 .-> CTRL
    M5 -. UART / CAN / RS485 .-> CTRL
    M6 -. UART / CAN / RS485 .-> CTRL
    M7 -. UART / CAN / RS485 .-> CTRL

    classDef module fill:#e8f4ff,stroke:#1d4ed8,stroke-width:1px,color:#111827
    classDef bus fill:#fff7ed,stroke:#c2410c,stroke-width:1px,color:#111827
    classDef power fill:#ecfdf5,stroke:#047857,stroke-width:1px,color:#111827
    classDef control fill:#f5f3ff,stroke:#6d28d9,stroke-width:1px,color:#111827
    classDef charger fill:#fefce8,stroke:#a16207,stroke-width:1px,color:#111827

    class M1,M2,M3,M4,M5,M6,M7 module
    class BUSP,BUSM bus
    class K,SHUNT,ROBOT power
    class CTRL control
    class CHG charger
```

## Notranja zgradba enega modula

```mermaid
flowchart LR

    CELLS["20700 Li-ion celice<br/>7S1P<br/>25.2 V nominalno<br/>29.4 V max<br/>75.6 Wh"]
    BMS["BMS 7S<br/>20–30 A<br/>balansiranje<br/>zaščite"]
    FUSE["Varovalka"]
    HOTSWAP["Hot-swap / precharge<br/>anti-spark<br/>zaščita proti povratnemu toku"]
    OUT["Izhod modula<br/>MOD+ / MOD-"]

    CELLS --> BMS
    BMS --> FUSE
    FUSE --> HOTSWAP
    HOTSWAP --> OUT

    BMS -. UART / CAN / RS485 .-> DIAG["Diagnostika"]

    classDef cells fill:#e8f4ff,stroke:#1d4ed8,stroke-width:1px,color:#111827
    classDef protection fill:#fee2e2,stroke:#b91c1c,stroke-width:1px,color:#111827
    classDef output fill:#ecfdf5,stroke:#047857,stroke-width:1px,color:#111827
    classDef control fill:#f5f3ff,stroke:#6d28d9,stroke-width:1px,color:#111827

    class CELLS cells
    class BMS,FUSE,HOTSWAP protection
    class OUT output
    class DIAG control
```
