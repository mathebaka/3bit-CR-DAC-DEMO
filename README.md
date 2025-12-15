# Description
3bit charge redistribution digital-to-analog converter demonstration for students of Eletronic Circuits 3 at TUeindhoven.  
<img width="1724" height="930" alt="CR DAC Demo - R02" src="https://github.com/user-attachments/assets/9eb0a148-1bae-4760-ae39-b8b1d1186a54" />

  
**Features**:
* Mechanical switches for manually changing the digital input code, resetting the DAC and turning off the device
* LED displays to show the digital input and analog output
* Battery-powered (one 18650 li-ion cell) and rechargeable (with over/under-discharge protection) through micro-USB (~5h on a single charge)
* The output of the voltmeter module (analog output) can be tuned using the trimmer potentiometer, allowing for:
  * Better accuracy
  * Option to change the displayed full-scale output of the DAC for demonstrative purposes
* Mounting holes for a small stand (easier to handle and use)

# Current Consumption
MAX Current Consumption (ignoring neglegible current from ICs):
* Node LEDs: 3 x 10mA = 30mA
* 7-segment - INPUT: 6 x 3 x 8mA = 144mA
* 7-segment - OUTPUT: 6 x 3 x 8mA + 8mA from DP = 152mA
* TOTAL: 326mA  

Assuming 90% efficiency from boost converter → Pout = Pin*0.9  
Vin → Nominal battery voltage = 3.6V  
  
Pout = Pin * 0.9  
326mA * 5V = 3.6V * I_battery * 0.9  
→ I_battery = 503.1mA  

Total battery capacity: 2500mAh  
=> Time with 1 full charge = 2500mAh / 503.1mA = 4.97h ≈ 5h  

# Internal Modules
Internal modules are sub-circuits within the PCB.
## Boost Converter 3.6V→5V
The TPS61322A IC was used in this module, the design is based off the second example in the [data sheet](https://www.ti.com/lit/ds/symlink/tps61322.pdf).
<img width="441" height="249" alt="{655755C6-4801-419E-B077-00EC6503CD4F}" src="https://github.com/user-attachments/assets/f562e5fe-b365-44eb-802b-61c11d2717c7" />  
* L1 = 2.2μH
* C1 = 10μF || 10μF || 10μF = 30μF
* D1: ZLLS410TA
* R1 = 5 Ohms
* C2 = 120pF

## Voltmeter Module
Centered around the [ICL7107](https://www.renesas.com/en/document/dst/icl7106-icl7107-icl7107s-datasheet?srsltid=AfmBOoq9oAzdzCN0QF71E_OcAmB2YEBGpXoNf32Zm5NzRzv7xGBHOHWO) chip, which is a dual-slope ADC with built-in 7-segment LED display driver circuitry. The circuit I built is the first example in their data sheet, the only difference is the added voltage divider for converting the DACs output to the max allowed input of the ICL7107 of 200mV; the specific values were chosen in order to main COUNT=500 at 5V, and Vref=100mV (from data sheet example).
<img width="377" height="456" alt="{ABEDFEF4-F0E7-4633-8FFC-4D323BDB8050}" src="https://github.com/user-attachments/assets/0285defa-6cff-44cd-a6e6-0228954590f3" />

# External Modules
Only one external module was used in release 2. 
## Li-ion Charger w/ Protection
TP4056 Micro-USB Li-ion charger 1A with Li-ion protection circuit ([link](https://www.tinytronics.nl/en/power/bms-and-chargers/li-ion-and-li-po/with-protection-circuit/tp4056-micro-usb-li-ion-charger-1a-with-li-ion-protection-circuit))
### Specifications:
* Charging current: 1A (maximum)
* Maximum charging voltage: 4.2V (1.5% accuracy)
* Input voltage: 4.5V-5.5V
* Two LED's indicating the status of charging (LED above R2 indicates charging, LED above R1 indicates when the battery is charged)

### Protection:
* over-discharge: >3A (too high current)
* under-discharge: <2.5V (too low voltage)
* Short-circuit?
* No temperature monitoring
  
<img width="1500" height="1500" alt="image" style="width:35%" src="https://github.com/user-attachments/assets/9e2c4fdd-06dc-46b1-b8ac-827cfa8e712d" />

# Schematic
<img src="images/schematic.svg" alt="schematic.svg" width="100%"/>

# PCB
PCB files:
* [KiCad Project](https://github.com/mathebaka/3bit-CR-DAC-DEMO/blob/main/KiCad%20files.zip)
* [Gerber files](https://github.com/mathebaka/3bit-CR-DAC-DEMO/blob/main/gerber.zip)

IMPORTANT: There are three noticeable mistakes in the PCB design
* 10uF capacitor at output of boost converter was assigned the wrong footprint → 0201 instead of 0603
* Header pin for the Li-ion charging module thinner than expected → use a thicker (breadboard thickness) instead for easier soldering
* Y-dimensions of Li-ion module are wrong (pins align in the x direction but not y) 

No components (bare):
<img width="914" height="801" alt="image" src="https://github.com/user-attachments/assets/8ec58d05-d66e-4cf6-8e8c-e4a3bd320819" />
<img width="915" height="799" alt="image" src="https://github.com/user-attachments/assets/35df073c-c26b-408f-b96e-c9dac4dee97a" />

With components:  
<img width="905" height="794" alt="image" src="https://github.com/user-attachments/assets/4f35c078-fa2b-4d08-88a2-48a04676608e" />
<img width="917" height="803" alt="image" src="https://github.com/user-attachments/assets/b56eca2e-c34c-4c9b-bc07-636654e97d75" />

PS: Switches 3D model pins are not the same as the ones from the actual switches used. The ones used in reality aligns with the header pin footprint.

# BOMs
You can find a PDF of the [BOMs](https://github.com/mathebaka/3bit-CR-DAC-DEMO/blob/main/BOMs.pdf) in the project folder, here is a table showing the same information:
| Side (T/B) | Item | Quantity | Value | Package |
|---|---|---:|---|---|
| T | Slide switch | 5 | - | - |
|  | Red LED | 3 | - | THT |
|  | Capacitor (Ceramic) | 8 | 1μF | THT |
|  | 7-segment LED display | 6 | - | THT |
|  | MCP6001 - rail-to-rail OP Amp | 1 | - | SOT-23-5 |
|  | Potentiometer | 1 | 5kΩ | THT |
|  | ICL7107CPL - ADC w/ LED driver | 1 | - | THT |
|  | 1s 18650 Li-ion charging module w/ protection | 1 | - | THT |
|  | Resistor (Thick Film) | 1 | 1MΩ | 1206 |
|  | ... | 1 | 22kΩ | 1206 |
| B | Stand-off + nut | 4 | M3 | - |
|  | 18650 Li-ion battery holder | 1 | - | SMD |
|  | 18650 Li-ion battery cell | 1 | - | - |
|  | TPS613222A - 5V boost converter | 1 | - | SOT-23-5 |
|  | Inductor | 1 | 2.2μH | SMD |
|  | ZLLS410TA - Diode | 1 | - | SOD323 |
|  | ICL7660 - Charge pump (converts +5V → -5V) | 1 | - | SOIC-8 |
|  | Capacitor (Electrolytic) | 2 | 100μF | SMD |
|  | Capacitor (Ceramic) | 1 | 120μF | 1206 |
|  | ... | 3 | 10μF | 0201 |
|  | ... | 1 | 100pF | 1206 |
|  | ... | 1 | 100nF | 1206 |
|  | ... | 1 | 470nF | 1206 |
|  | ... | 1 | 220nF | 1206 |
|  | ... | 1 | 10nF | 1206 |
|  | Resistor (Thick Film) | 19 | 470Ω | 1206 |
|  | ... | 3 | 330Ω | 1206 |
|  | ... | 1 | 100kΩ | 1206 |
|  | ... | 1 | 20kΩ | 1206 |
|  | ... | 1 | 390Ω | 1206 |
|  | ... | 1 | 47kΩ | 1206 |

# R02 Checklist
**Power Supply**:
Lower complexity + More compact
- [X] Scrapped +10.6V source → everything on the board is powered by 5V now
- [X] 3 Li-ion 18650 batteries → 1 Li-ion 18650 battery

**External Modules**:
3 external modules → 1 external module
- [X] Voltmeter module → in-board voltmeter [own design w/ SMD components]
- [X] Buck-converter module → in-board boost-converter (3.6V to 5V) [own design w/ SMD components]
- [X] 3s BMS & 3s USB-C charger → 1s USB-C charger with protection

**Components**:
- [X] LM358 → MCP6001 rail-to-rail input/output (RRIO)
- [X] THT → SMD for all components except DAC capacitors (for demonstration purposes)

**PCB**:
- [X] New PCB design required to accommodate new changes

**FINAL CHANGES**:
- [X] Round corners
- [X] Logo on B-side

**README**:
- [X] BOMs
