# RTL8370N-hacking

My tools collection for activating VLANS support on dumb switchs powered by the RTL8370N

### To add VLAN support to your switch set registers (in the EEPROM 27c16): 

- Make a dump of you EEPROM.
- Open your newly created dump file with the tool provided (`Javascript tool\Index.htm`)
- Add necessary registers (provided below), save to a new ROM and burn it.
- Enjoy !
- NOTE : You can find my ROM here : [ROM Modified](https://github.com/botzunga/RTL8370N-hacking/raw/refs/heads/main/EEPROM-24C16/DLINK_DGS-1008MP_VLAN_PORT01U_V3-PORT234567U_V2-PORT1T_V2.bin), [ROM Original](https://github.com/botzunga/RTL8370N-hacking/raw/refs/heads/main/EEPROM-24C16/DLINK_DGS-1008MP_original.bin)

#### ▶️ Enable support for VLAN traffic ingress switch wide 
| Register HEX | Register name                    | Value | Description                       |
|--------------|----------------------------------|-------|-----------------------------------|
| `0x07A9  `     | VLAN_INGRESS                     | `0x00FF`| Enable VLAN traffic ingress       |


#### ▶️ Set PVID VLAN for each port 
| Register HEX | Register name                    | Value | Description                       |
|--------------|----------------------------------|-------|-----------------------------------|
| `0x072B`       | VLAN_MEMBER_CONFIGURATION0_CTRL3 | `0x03`  | Set PVID VLAN for port 0 on VLAN 3|
| `0x072F`       | VLAN_MEMBER_CONFIGURATION1_CTRL3 | `0x03`  | Set PVID VLAN for port 1 on VLAN 3|
| `0x0733`       | VLAN_MEMBER_CONFIGURATION2_CTRL3 | `0x02`  | Set PVID VLAN for port 2 on VLAN 2|
| `0x0737`       | VLAN_MEMBER_CONFIGURATION3_CTRL3 | `0x02`  | Set PVID VLAN for port 3 on VLAN 2|
| `0x073B`       | VLAN_MEMBER_CONFIGURATION4_CTRL3 | `0x02`  | Set PVID VLAN for port 4 on VLAN 2|
| `0x073F`       | VLAN_MEMBER_CONFIGURATION5_CTRL3 | `0x02`  | Set PVID VLAN for port 5 on VLAN 2|
| `0x0743`       | VLAN_MEMBER_CONFIGURATION6_CTRL3 | `0x02`  | Set PVID VLAN for port 6 on VLAN 2|
| `0x0747`       | VLAN_MEMBER_CONFIGURATION7_CTRL3 | `0x02`  | Set PVID VLAN for port 7 on VLAN 2|
 

#### ▶️ Enable CVLAN filtering (else all tagged VLANS goes on all ports)
| Register HEX | Register name                    | Value | Description                       |
|--------------|----------------------------------|-------|-----------------------------------|
| `0x07A8`       | VLAN_CTRL                        | `0x0001`| global ; Enable CVLAN filtering   |

 #### ▶️ Define your VLAN configuration
##### For VLAN 3 : 
| Register HEX | Register name                    | Value | Description                       |
|--------------|----------------------------------|-------|-----------------------------------|
| `0x0510`     | TABLE_ACCESS_DATA13              | `0x0303`| Configure port bitmask ; Set port 0 and 1 member of VLAN 3 UNTAGGED and set port 0 and 1             |
| `0x0501`     | TABLE_ACCESS_ADDR                | `0x0003`| Set VLAN ID member to 3           |
| `0x0500`     | TABLE_ACCESS_CTRL                | `0x000B`| Commit changes                    |
##### For VLAN 2 : 
| Register HEX | Register name                    | Value | Description                       |
|--------------|----------------------------------|-------|-----------------------------------|
| `0x0510`       | TABLE_ACCESS_DATA13              | `0xFCFE`| Configure port bitmask ; Set port 2 to 7 member of VLAN 2 UNTAGGED, and set port 1 to 7 member of VLAN 2|
| `0x0501`       | TABLE_ACCESS_ADDR                | `0x0002`| Set VLAN ID member to 2           |
| `0x0500`       | TABLE_ACCESS_CTRL                | `0x000B`| Commit changes                    |
### Now, your switch must be in this VLAN configuration : 
| Port   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|--------|---|---|---|---|---|---|---|---|
| Vlan 2 |   | T | U | U | U | U | U | U |
| Vlan 3 | U | U |   |   |   |   |   |   |

(U for UNTAG, T for TAG)

 ---
### Heres is the listing of all registers modified : 
| Register HEX | Register name                    | Value | Description                       |
|--------------|----------------------------------|-------|-----------------------------------|
| `0x07A9  `     | VLAN_INGRESS                     | `0x00FF`| Enable VLAN traffic ingress       |
| `0x072B`       | VLAN_MEMBER_CONFIGURATION0_CTRL3 | `0x03`  | Set PVID VLAN for port 0 on VLAN 3|
| `0x072F`       | VLAN_MEMBER_CONFIGURATION1_CTRL3 | `0x03`  | Set PVID VLAN for port 1 on VLAN 3|
| `0x0733`       | VLAN_MEMBER_CONFIGURATION2_CTRL3 | `0x02`  | Set PVID VLAN for port 2 on VLAN 2|
| `0x0737`       | VLAN_MEMBER_CONFIGURATION3_CTRL3 | `0x02`  | Set PVID VLAN for port 3 on VLAN 2|
| `0x073B`       | VLAN_MEMBER_CONFIGURATION4_CTRL3 | `0x02`  | Set PVID VLAN for port 4 on VLAN 2|
| `0x073F`       | VLAN_MEMBER_CONFIGURATION5_CTRL3 | `0x02`  | Set PVID VLAN for port 5 on VLAN 2|
| `0x0743`       | VLAN_MEMBER_CONFIGURATION6_CTRL3 | `0x02`  | Set PVID VLAN for port 6 on VLAN 2|
| `0x0747`       | VLAN_MEMBER_CONFIGURATION7_CTRL3 | `0x02`  | Set PVID VLAN for port 7 on VLAN 2|
| `0x07A8`       | VLAN_CTRL                        | `0x0001`| global ; Enable CVLAN filtering   |
| `0x0510`       | TABLE_ACCESS_DATA13              | `0x0303`| Configure port bitmask ; Set port 0 and 1 member of VLAN 3 UNTAGGED and set port 0 and 1             |
| `0x0501`       | TABLE_ACCESS_ADDR                | `0x0003`| Set VLAN ID member to 3           |
| `0x0500`       | TABLE_ACCESS_CTRL                | `0x000B`| Commit changes                    |
| `0x0510`       | TABLE_ACCESS_DATA13              | `0xFCFE`| Configure port bitmask ; Set port 2 to 7 member of VLAN 2 UNTAGGED, and set port 1 to 7 member of VLAN 2|
| `0x0501`       | TABLE_ACCESS_ADDR                | `0x0002`| Set VLAN ID member to 2           |
| `0x0500`       | TABLE_ACCESS_CTRL                | `0x000B`| Commit changes                    |

 ---
 ## Note :
 The register `0x0510` "TABLE_ACCESS_DATA13" is 16 bits wide.
 The LSB byte of this register determines if the port is member of the VLAN (TAGGED)
 The MSB byte of this register determines if the port is member of the VLAN (UNTAGGED)
 
 Example : 
``` 
     |UNTAGGED| TAGGED |
 MSB | ports  |  ports | LSB
   0b hgfedcba HGFEDCBA
 ```
 -> Port (A or a) is the physical port 0, (B or b) is the physical port 1 and so on...
 -> Letters in **uppercase** represents the bit port in a TAGGED vlan  
 -> Letters in **lowercase** represents the bit port in a UNTAGGED vlan
 
 - `0b0000111100001111 (0x0F0F)` sets the port 0 to 3 to VLAN member UNTAGGED.
 - `0b0000111111111111 (0xFF0F)` sets the port 0 to 3 to VLAN member UNTAGGED **and** sets the port 0 to 7 to VLAN member TAGGED
 - `0b0000000011111111 (0x00FF)` sets the port 0 to 7 to VLAN member TAGGED.
  (Note : `0x0501` "TABLE_ACCESS_ADDR" sets the VLANID member)
  
  
  A BIG thanks for : 
  - @renzenicolai, for your javascript tool
  - @Sprite_TM, for your work on a AVR based console ( https://spritesmods.com/?art=rtl8366sb&page=2 ) 
  - @Bercik, for your work on a AVR based console ( https://web.archive.org/web/20140824233323if_/http://bercik.wel.wat.edu.pl/switch_hack_by_Bercik.zip )
  
 @Alex 2025/02/19

