# FARMBEAST modularni baterijski sistem

Spodnji diagram prikazuje vseh 7 baterijskih modulov kot samostojne 7S1P enote. Vsak modul ima svoj BMS, varovalko, hot-swap/precharge stopnjo in izhod na skupni DC vod.

```mermaid
flowchart LR

    %% FARMBEAST modularni baterijski sistem
    %% 7x modul 7S1P 20700 Li-ion v paraleli

    subgraph M1["Modul 1 — 7S1P 20700 Li-ion"]
        C1["7 celic v seriji<br/>25.2 V nominalno<br/>29.4 V max<br/>75.6 Wh"]
        BMS1["BMS 7S<br/>20–30 A<br/>balansiranje + zaščite"]
        F1["Varovalka"]
        HS1["Hot-swap / precharge stopnja<br/>anti-spark<br/>zaščita proti povratnemu toku"]
        OUT1["Izhod modula<br/>MOD+ / MOD-"]

        C1 --> BMS1
        BMS1 --> F1
        F1 --> HS1
        HS1 --> OUT1
    end

    subgraph M2["Modul 2 — 7S1P 20700 Li-ion"]
        C2["7 celic v seriji<br/>25.2 V nominalno<br/>29.4 V max<br/>75.6 Wh"]
        BMS2["BMS 7S<br/>20–30 A<br/>balansiranje + zaščite"]
        F2["Varovalka"]
        HS2["Hot-swap / precharge stopnja<br/>anti-spark<br/>zaščita proti povratnemu toku"]
        OUT2["Izhod modula<br/>MOD+ / MOD-"]

        C2 --> BMS2
        BMS2 --> F2
        F2 --> HS2
        HS2 --> OUT2
    end

    subgraph M3["Modul 3 — 7S1P 20700 Li-ion"]
        C3["7 celic v seriji<br/>25.2 V nominalno<br/>29.4 V max<br/>75.6 Wh"]
        BMS3["BMS 7S<br/>20–30 A<br/>balansiranje + zaščite"]
        F3["Varovalka"]
        HS3["Hot-swap / precharge stopnja<br/>anti-spark<br/>zaščita proti povratnemu toku"]
        OUT3["Izhod modula<br/>MOD+ / MOD-"]

        C3 --> BMS3
        BMS3 --> F3
        F3 --> HS3
        HS3 --> OUT3
    end

    subgraph M4["Modul 4 — 7S1P 20700 Li-ion"]
        C4["7 celic v seriji<br/>25.2 V nominalno<br/>29.4 V max<br/>75.6 Wh"]
        BMS4["BMS 7S<br/>20–30 A<br/>balansiranje + zaščite"]
        F4["Varovalka"]
        HS4["Hot-swap / precharge stopnja<br/>anti-spark<br/>zaščita proti povratnemu toku"]
        OUT4["Izhod modula<br/>MOD+ / MOD-"]

        C4 --> BMS4
        BMS4 --> F4
        F4 --> HS4
        HS4 --> OUT4
    end

    subgraph M5["Modul 5 — 7S1P 20700 Li-ion"]
        C5["7 celic v seriji<br/>25.2 V nominalno<br/>29.4 V max<br/>75.6 Wh"]
        BMS5["BMS 7S<br/>20–30 A<br/>balansiranje + zaščite"]
        F5["Varovalka"]
        HS5["Hot-swap / precharge stopnja<br/>anti-spark<br/>zaščita proti povratnemu toku"]
        OUT5["Izhod modula<br/>MOD+ / MOD-"]

        C5 --> BMS5
        BMS5 --> F5
        F5 --> HS5
        HS5 --> OUT5
    end

    subgraph M6["Modul 6 — 7S1P 20700 Li-ion"]
        C6["7 celic v seriji<br/>25.2 V nominalno<br/>29.4 V max<br/>75.6 Wh"]
        BMS6["BMS 7S<br/>20–30 A<br/>balansiranje + zaščite"]
        F6["Varovalka"]
        HS6["Hot-swap / precharge stopnja<br/>anti-spark<br/>zaščita proti povratnemu toku"]
        OUT6["Izhod modula<br/>MOD+ / MOD-"]

        C6 --> BMS6
        BMS6 --> F6
        F6 --> HS6
        HS6 --> OUT6
    end

    subgraph M7["Modul 7 — 7S1P 20700 Li-ion"]
        C7["7 celic v seriji<br/>25.2 V nominalno<br/>29.4 V max<br/>75.6 Wh"]
        BMS7["BMS 7S<br/>20–30 A<br/>balansiranje + zaščite"]
        F7["Varovalka"]
        HS7["Hot-swap / precharge stopnja<br/>anti-spark<br/>zaščita proti povratnemu toku"]
        OUT7["Izhod modula<br/>MOD+ / MOD-"]

        C7 --> BMS7
        BMS7 --> F7
        F7 --> HS7
        HS7 --> OUT7
    end

    subgraph BUS["Skupni DC vod"]
        BUSP["+ DC BUS"]
        BUSM["- DC BUS"]
    end

    OUT1 --> BUSP
    OUT1 --> BUSM

    OUT2 --> BUSP
    OUT2 --> BUSM

    OUT3 --> BUSP
    OUT3 --> BUSM

    OUT4 --> BUSP
    OUT4 --> BUSM

    OUT5 --> BUSP
    OUT5 --> BUSM

    OUT6 --> BUSP
    OUT6 --> BUSM

    OUT7 --> BUSP
    OUT7 --> BUSM

    BUSP --> K["Glavni kontaktor / DC odklopnik"]
    K --> ROBOTP["Robot + vhod"]

    BUSM --> SHUNT["Merilni shunt / tokovni senzor"]
    SHUNT --> ROBOTM["Robot - vhod"]

    ROBOTP --> ROBOT["FARMBEAST robot<br/>DC vhod: 21–29.4 V<br/>maks. tok: 60 A"]
    ROBOTM --> ROBOT

    CHG["Polnilec / servisna postaja<br/>7S Li-ion CC/CV<br/>končna napetost: 29.4 V"]

    CHG --> BUSP
    CHG --> BUSM

    BMS1 -. UART / CAN / RS485 .-> CTRL["Diagnostika / krmilnik<br/>napetosti členov<br/>tok<br/>temperatura<br/>napake"]
    BMS2 -. UART / CAN / RS485 .-> CTRL
    BMS3 -. UART / CAN / RS485 .-> CTRL
    BMS4 -. UART / CAN / RS485 .-> CTRL
    BMS5 -. UART / CAN / RS485 .-> CTRL
    BMS6 -. UART / CAN / RS485 .-> CTRL
    BMS7 -. UART / CAN / RS485 .-> CTRL

    style ROBOT fill:#e8f4ff,stroke:#333,stroke-width:1px
    style CHG fill:#fff4e6,stroke:#333,stroke-width:1px
    style CTRL fill:#f3e8ff,stroke:#333,stroke-width:1px
```

## Opomba

To je konceptni blokovni diagram. Ni prava električna oziroma PCB shema. Za dejansko izvedbo je treba izbrati konkretne komponente: BMS, varovalke, konektorje, kontaktor, precharge upor, MOSFET/ideal-diode hot-swap stopnjo, merilnik toka in komunikacijski vmesnik.