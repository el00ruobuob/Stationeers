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

define item -404336834 #Copper
#define item 226410516 #Gold
#define item -1301215609 #Iron
#define item 2134647745 #Lead
#define item -1406385572 #Nickel
#define item -290196476 #Silicon
#define item -929742000 #Silver
#define item 1058547521 #Constantan
#define item 502280180 #Electrum
#define item -297990285 #Invar
#define item -82508479 #Solder
#define item -654790771 #Steel
#define item 412924554 #Astroloy
#define item -787796599 #Inconel
#define item -1897868623 #Stellite
#define item 156348098 #Waspaloy