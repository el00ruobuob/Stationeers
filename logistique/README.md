## Request IC variable

Formated as QQQQQ.II.RR.MM.SS.FF (Formaly MM.QQQQ.I.R.S.F)
- QQQQQ: Material Quantity on 5 Digit (up to 99,999kg)
- II: Info seeker, as which sorting device needs rooting info
- RR: Requester, as in which printer has ordered material
- MM: Type of materials
- SS: Delivery Status
- FF: Flag


### Type of material
MM | old MM | threshold | Hash | Type | Material
-- | -- | -- | ---- | ---- | --------
01 | 01 |  | 252561409 | Ore | Charcoal
02 | 02 |  | 1724793494 | Ore | Coal
03 | 03 |  | -983091249 | Ore | Cobalt
04 | 04 |  | -707307845 | Ore | Copper
05 | 05 |  | -1348105509 | Ore | Gold
06 | 06 |  | 1758427767 | Ore | Iron
07 | 07 |  | -190236170 | Ore | Lead
08 | 08 |  | 1830218956 | Ore | Nickel
09 | 09 |  | 1103972403 | Ore | Silicon
10 | 10 |  | -916518678 | Ore | Silver
11 | 11 |  | -1516581844 | Ore | Uranium
12 | 12 |  | -831480639 | Ore | Biomass
-- | -- |  | ---- | ---- | --------
14 | 21 | 10 | -404336834 | Ingot | Copper
15 | 22 | 8 | 226410516 | Ingot | Gold
16 | 23 | 10 | -1301215609 | Ingot | Iron
17 | 24 | 1 | 2134647745 | Ingot | Lead
18 | 25 | 1 | -1406385572 | Ingot | Nickel
19 | 26 | 8 | -290196476 | Ingot | Silicon
20 | 27 | 1 | -929742000 | Ingot | Silver
-- | -- |  | ---- | ---- | --------
22 | 31 | 5 | 1058547521 | Alloy | Constantan
23 | 32 | 8 | 502280180 | Alloy | Electrum
24 | 33 | 5 | -297990285 | Alloy | Invar
25 | 34 | 5 | -82508479 | Alloy | Solder
26 | 35 | 10 | -654790771 | Alloy | Steel
-- | -- |  | ---- | ---- | --------
28 | 41 | 4 | 412924554 | Advanced Alloy | Astroloy
29 | 41 | 1 | 1579842814 | Advanced Alloy | Hastelloy
30 | 42 | 4 | -787796599 | Advanced Alloy | Inconel
31 | 43 | 4 | -1897868623 | Advanced Alloy | Stellite
32 | 44 | 2 | 156348098 | Advanced Alloy | Waspaloy

### Info Seeker

On 2 digits, device requesting information from logistic controller:
* 00: Query
* 01: Stacker
* 02: SorterLB
* 03: SorterFurn
* 04: SorterTool
* 05: SorterLathe
* 06: SorterElec
* 07: SorterBender
* 08: SorterGenerator
  
### Requester

On 2 digits, device requesting material (printer, furnace, etc.)
* 00: Query
* 01: None
* 02: Furnace
* 03: Tool
* 04: Lathe
* 05: Elec
* 06: Bender
* 07: Coal Generator

### Status

On 2 digits, Status of delivery
* 00: query
* 01: demand
* 02: pulling
* 03: pulled
* 04: Quantified
* 05: routed
* 09: Error

### Communication Flag

On 2 digits, Communication flag for data Acknoledgement (tcp-inspired)
* 01: SYN
* 02: SYN/ACK
* 03: ACK

# Comm method

1) Any device wanting to write to the ReqIC loads ReqIC setting value  
  1.1) If ReqIC is 0, it moves on to step 2  
  1.2) If ReqIC is not 0, restart to step 1
2) The device set ReqIC setting to the needed value to request for ingot, ask for an existing request, or update a request, with F set with 1
3) The device reads the reqIC again, until the value has changed.  
  3.1) If the value has only increased the F to 2, it moves on to step 4  
  3.2) If the value has changed to anything else, it goes back to step 1  
4) The device writes back the value, with F set to 3 to ReqIC

# ReqIC

1) ReqIC check its setting value  
    1) If setting is 0, goes back to step 1  
    2) If setting is not 0, move to step 2  
2) If I == 0  
    1) if S == 1:  
        1) add to stack  
        2) set back with F = 2  
        3) wait for F == 3  
        4) set back with S = 2 and F = 1  
        5) wait for F == 2  
        6) set back with F = 3  (Silo will wait for F == 3 or 0 to set back with S = 3 and F = 1)  
3) If I != 0
    1) If R && S != 0
        1) Search stack with == MM, == R & <= S
        2) If S < 5
            1) Update stack with received value
        3) if S == 5:  
            1) delete from stack  
        4) Set back with F = 2
        5) Wait fo F == 3
    2) If Q || R == 0
        1) Search stack with MM
        2) Take the first (oldest) and set back with F = 2
        3) Wait for F == 3
4) If F == 3
    1) Set back to 0

## Pannels
### ReqIC pannels

```
Automation variable:
* QQQQQ.II.RR.MM.SS.FF

Data are multiplexed on 15 digits
```
```
MM: 2 Digits, Type of material
* 01-12: Ores, CharCoal -> Biomass
* 14-32: Ingots, alloys
* 14-20: Copper - Silver
* 22-26: Constantan - Steel
* 28-32: Astroloy - Waspaloy
```
```
QQQQQ: 5 digits, Quantity of said material
```
```
II: On 2 digits, device requesting information from logistic controller:
* 00: Query
* 01: Stacker
* 02: SorterLB
* 03: SorterFurn
* 04: SorterTool
* 05: SorterLathe
* 06: SorterElec
* 07: SorterBender
* 08: SorterGenerator
```
```
RR: On 2 digits, device requesting material (printer, furnace, etc.)
* 00: Query
* 01: None
* 02: Furnace
* 03: Tool
* 04: Lathe
* 05: Elec
* 06: Bender
* 07: Coal Generator
```
```
SS: On 2 digits, Status of delivery
* 00: query
* 01: demand
* 02: pulling
* 03: pulled
* 04: Quantified
* 05: routed
* 09: Error
```
```
FF: On 2 digits, Communication flag for data Acknoledgement
* 01: SYN
* 02: SYN/ACK
* 03: ACK
Last Ack is always done from reqIC (3 is not read by device - here to allow overwriting)
```
