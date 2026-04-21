# Inktime_TSC_Project

## Cuprins:

1. Hardware: - Schematicul si pcb-ul
2. Manufacturing: - Bill of Materials si Pick & Place
3. Mechanical: fisierele 3D
4. Images: Imaginile cu produsul final

## Descriere
InkTime este un ceas smart optimizat pentru autonomie maximă și lizibilitate 
în lumina soarelui. MVP-ul include:

- Display e-paper 1.54" (200×200) cu refresh parțial o dată pe minut
- Notificări BLE de pe telefon cu alertă haptic
- Numărare pași în timp real prin accelerometru cu pedometru integrat
- Încărcare USB-C și programare prin Tag-Connect TC2030 SWD

## Arhitectură
Sistemul este construit în jurul MCU-ului **nRF52840** care integrează 
procesor ARM Cortex-M4 și radio BLE 5.0.
Alimentarea este asigurată de un charger LiPo **BQ25180** conectat la 
USB-C și un regulator buck-boost **RT6160** care menține 3.3V stabil 
pe toată gama de descărcare a bateriei.

## Schema Bloc
![Block Diagram](images/block_diagram.png)

## Bill of Materials
| Ref | Component | Package | Funcție | LCSC | Datasheet |
|-----|-----------|---------|---------|------|-----------|
| U1 | nRF52840-QIAA | aQFN73 7×7mm | MCU + BLE 5.0 | [C190794](https://www.lcsc.com/product-detail/C190794.html) | [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.7.pdf) |
| IC1 | BQ25180YBGR | DSBGA-8 | Charger USB-C LiPo | [C3682423](https://www.lcsc.com/product-detail/C3682423.html)| [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf) |
| IC9 | RT6160AWSC | WLCSP-15 | Regulator 3.3V buck-boost | [C7065276](https://www.lcsc.com/product-detail/C7065276.html) | [Datasheet](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-00.pdf) |
| U3 | MAX17048G+T10 | DFN-8 2×2mm | Fuel gauge LiPo | [C2682616](https://www.lcsc.com/product-detail/C2682616.html) | [Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/MAX17048-MAX17049.pdf) |
| IC3 | BMA421 | LGA-12 2×2mm | Accelerometru + pedometru | [C5242966](https://www.lcsc.com/product-detail/C5242966.html) | [Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bma421-ds000.pdf) |
| IC2 | DRV2605LDGSR | VSSOP-10 | Haptic driver ERM | [C527464](https://www.lcsc.com/product-detail/C527464.html) | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605l.pdf) |
| D3 | USBLC6-2SC6Y | SOT-363 | Protecție ESD USB | [C2687116](https://www.lcsc.com/product-detail/C2687116.html) | [Datasheet](https://www.st.com/resource/en/datasheet/usblc6-2.pdf) |
| Q1 | DMG2305UX-7 | SOT-323 | PFET EPD power gate | [C370432](https://www.lcsc.com/product-detail/C370432.html) | [Datasheet](https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf) |
| Q3 | SI2301CDS | SOT-23 | PFET EPD gate control | [C10487](https://www.lcsc.com/product-detail/C10487.html) | [Datasheet](https://www.vishay.com/docs/70763/si2301cds.pdf) |
| J4 | KH-TYPE-C-16P | SMD | Conector USB-C | [C165948](https://www.lcsc.com/product-detail/C165948.html) | [Datasheet](https://datasheet.lcsc.com/lcsc/1912111437_Korean-Hroparts-Elec-TYPE-C-31-M-12_C165948.pdf) |
| J1 | 503480-2400 | SMD FPC | Conector display EPD 24 pini | [C393234](https://www.lcsc.com/product-detail/C393234.html) | [Datasheet](https://www.molex.com/pdm_docs/sd/5034802400_sd.pdf) |
| J2 | TC2030-IDC | PTH | Conector debug SWD Tag-Connect | - | [Datasheet](https://www.tag-connect.com/wp-content/uploads/bsk-pdf-manager/TC2030-IDC_1.pdf) |
| ANT1 | 2450AT18B100E | SMD | Antenă 2.4GHz BLE | - | [Datasheet](https://www.johansontechnology.com/datasheets/2450AT18B100E/2450AT18B100E.pdf) |
| L5 | WE-TPC 744042680 | 4828 | Inductor 68µH EPD boost | - | [Datasheet](https://www.we-online.com/catalog/datasheet/744042680.pdf) |
| L7 | FTC252012SR47MBCA | 0603 | Inductor 0.47µH RT6160 | - | - |
| X1 | NT2016SA-32MHz | 3225 | Crystal 32MHz MCU | - | - |
| X2 | Crystal 32.768kHz | 3215 | Crystal RTC | - | - |
| D2, D4, D5 | MBR0530T1G | SOD-123 | Diodă Schottky EPD driver | - | [Datasheet](https://www.onsemi.com/pdf/datasheet/mbr0530t1-d.pdf) |


## Pinii nRF52840

| Pin | Semnal | Componentă | Interfață |
|-----|--------|------------|-----------|
| P0.00/XL1 | XL1 | X2 32.768kHz crystal | Crystal RTC |
| P0.01/XL2 | XL2 | X2 32.768kHz crystal | Crystal RTC |
| P0.02 | SCK | EPD display | SPI |
| P0.03 | MOSI | EPD display | SPI |
| P0.05 | EPD_CS | EPD display | SPI CS |
| P0.06 | SDA | I2C bus (toți senzorii) | I2C |
| P0.07 | SCL | I2C bus (toți senzorii) | I2C |
| P0.08 | IMU_INT1 | BMA421 | GPIO interrupt |
| P0.12 | HAPTIC_EN | DRV2605L | GPIO enable |
| P0.13 | SW_UP | Buton UP | GPIO active-low |
| P0.14 | SW_DN | Buton DOWN | GPIO active-low |
| P0.15 | EPD_DC | EPD display | GPIO data/cmd |
| P0.16 | EPD_RST | EPD display | GPIO reset |
| P0.17 | EPD_BUSY | EPD display | GPIO busy |
| P0.18/RESET | RESET | TC2030 J2 | Hardware reset |
| P0.24 | SWDCLK | TC2030 J2 | SWD debug |
| P0.25 | SWDIO | TC2030 J2 | SWD debug |
| P1.00 | SW_ENT | Buton ENTER | GPIO active-low |
| P1.01 | EPD_3V3 gate | Q1 PFET | GPIO power gate EPD |
| P1.08 | IMU_INT2 | BMA421 | GPIO interrupt |
| VBUS | VBUS | J4 USB-C | USB 5V detect |
| D+ | DP | J4 USB-C | USB data+ |
| D- | DN | J4 USB-C | USB data- |
| ANT | RF | ANT1 2450AT18B100E | 2.4GHz BLE |

## Funcționalitate Hardware

### Power Path
USB-C (5V) → BQ25180 (charger + power path) → RT6160 (buck-boost 3.3V) → toți senzorii și MCU.
LiPo conectat la BQ25180 BAT. MAX17048 măsoară nivelul bateriei prin VBAT.
RT6160 menține 3.3V stabil pe toată gama de descărcare a bateriei (4.2V - 3.0V).

### MCU – nRF52840
Microcontroller ARM Cortex-M4 cu FPU, 64MHz, 1MB flash, 256KB RAM.
BLE 5.0 integrat cu antenă externă 2450AT18B100E plasată la exteriorul PCB-ului cu keepout sub ea.
Programare prin Tag-Connect TC2030 SWD (SWDIO, SWDCLK, RESET).

### Display E-Paper
Conector FPC 24 pini (J1) pentru panou e-paper 1.54" 200x200.
Comunicație SPI: SCK P0.02, MOSI P0.03, CS P0.05, DC P0.15, RST P0.16, BUSY P0.17.
Alimentare VEPD controlată prin PFET Q1 (gate P1.01) pentru power gating.
Circuit driver EPD: inductor L5 68µH, diode Schottky D2/D4/D5, condensatoare boost.

### IMU – BMA421
Accelerometru 3 axe cu pedometru integrat hardware.
Interfață I2C (adresă 0x18), INT1 pe P0.08 pentru wake-on-motion, INT2 pe P1.08.

### Haptic – DRV2605L
Driver motor ERM cu biblioteca de efecte haptic integrate.
Interfață I2C (adresă 0x5A), enable pe P0.12.
Motor ERM conectat direct la OUT+ și OUT-.

### Butoane
3 butoane tactile: UP (P0.13), DOWN (P0.14), ENTER (P1.00).
Configurație active-low cu pull-up intern activat în firmware.

### USB
Conector USB-C cu protecție ESD USBLC6 pe liniile D+/D-.
VBUS conectat la BQ25180 și la pinul de detecție al nRF52840.

### I2C Bus
Shared I2C pe SDA P0.06 / SCL P0.07 la 400kHz.
Dispozitive: BQ25180 (0x6A), RT6160 (0x75), MAX17048 (0x36), BMA421 (0x18), DRV2605L (0x5A).
Pull-up 10kΩ la 3.3V.


## PCB Design Notes

### Stackup
4 layere:
- Layer 1 (Top): semnal + componente SMD
- Layer 2: GND plane
- Layer 3: 3.3V power plane
- Layer 4 (Bottom): semnal

### Decizii de design
- GND rezolvat prin polygon pour pe toate layerele (Top, c2, Bottom)
- 3V3 rezolvat prin polygon pour pe layer-ul c63
- Antena 2450AT18B100E plasată la exteriorul boardului cu decupaj (keepout) 
  sub elementul radiant – fără cupru, fără trasee, fără plane sub antenă
- Condensatoare de decuplare plasate lângă pinii VCC ai fiecărui IC
- BMA421 plasat departe de motorul ERM pentru a evita false step counts
- Trasee de semnal: 0.15mm, trasee de power: 0.3mm

### Erori acceptate în DRC
- **Overlap Via-Smd - 6 erori approved**: via-uri în zona interioară a nRF52840 QFN73 
  din cauza densității extreme a pad-urilor. Nu pot fi rutate altfel in versiunea mea de rutare.
- **Copper Width - 2 erori approved**: trasee înguste (0.1mm) folosite pentru escape routing 
  din zona pinilor interiori ai MCU-ului unde spațiul nu permite 0.15mm.
- **Board Outline Clearance**: cauzate de modelul 3D al conectorului USB-C 
  care depășește ușor outline-ul – nu afectează fabricația.
- **Pad-Solid Polygon Shape, overlap**: footprintul pentru switchuri pe care l-am primit este facut prost.

### Probleme cunoscute
- Pinul RESET al nRF52840 rutat prin via escape din cauza
  poziției interioare în pachetul QFN73.
- Rutarea este foarte densă în zona nRF52840 din cauza 
  numărului mare de pini (73) și a spațiului limitat pe board.
  Acest lucru a necesitat trasee de 0.1mm și via-uri de escape
  pentru pinii din rândurile interioare ale chipului.
