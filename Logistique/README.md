# Variable

MMQQQQIRSF

* MM: 2 Digits, Type of material
* QQQQ: 4 digits, Quantity of said material
* I: 1 Digit, Device needing information
  * 0: Query
  * 1: Stacker
  * 2: SorterLB
  * 3: SorterFurn
  * 4: SorterTool
  * 5: SorterLathe
  * 6: SorterElec
  * 7: SorterBender (not used yet)
* R: 1 Digit, Requester (printer, furnace)
  * 0: Query
  * 1: None
  * 2: Furnace
  * 3: Tool
  * 4: Lathe
  * 5: Elec
  * 6: Bender
* S: 1 Digit, Status of delivery
  * 0: query
  * 1: demand
  * 2: pulling
  * 3: pulled
  * 4: Quantified
  * 5: routed
  * 9: Error
* F: 1 Digit, Ack status
  * 1: SYN
  * 2: SYNACK
  * 3: ACK

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