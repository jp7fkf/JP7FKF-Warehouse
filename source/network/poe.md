# PoE

## 規格
### IEEE
- 802.3af
- 802.3at
- 802.3bt

## Keywords
- PSE: Power sourcing equipment
- PD: Powered device

## IEEE 802.3at-2009 (as known as  oE+, PoEP (?))
- cat 5e or more required.
- class0(Type-1): Min PSE Power 15.4W, Max PD Power 12.95W (0.44–12.95 W at the PD; default classification)
- class1(Type-1): Min PSE Power 4W, Max PD Power 3.84W (0.44–3.84 W at the PD)
- class2(Type-1): Min PSE Power 7W, Max PD Power 6.49W (3.84–6.49 W at the PD)
- class3(Type-1): Min PSE Power 15.4W, Max PD Power 12.95W
- class4(Type-2): Min PSE Power 30.0W, Max PD Power 25.5W (12.95 W to 25.50 W at the PD).

- back compatible with IEEE802.3af-2003
- Type1: Low power, Type2: High power
  - Cabling: CAT5E required
  - Voltage: PSE voltage from 44V to 57V (50V to 57V for Type-2 PSEs)
  - Current level: 600mA assuming cable temperature is 50C or lower
  - Polarity: End-Spans can use MDI or MDI-X (Positive or Negative Polarity)
  - Pulse transformer inductance:
– End-Span PSE (Switch) & PDs: 120µH (allows usage of IEEE802.3af magnetics)
– Mid-Span: 350µH, Midspan Alternative-A 10/100 installations require regulation
  - Power Feeding: 2-pairs and 4-pairs possible
– Focus on 2-pairs Medium Power: PSE 30W output, PD 25.5W input
– 4-pairs High Power should be based on 2x2-pairs: PSE 60W output, PD 51W input 

- Type1ではPDのL1 negotiation(Classification) だけで可能(ほかはOptional)だが，Type2ではPDのL1, L2 negotiation, およびPSEのL1 negotiationが必須．

## IEEE 802.3af
- Cat 3 or more required.
- IEEE 802.3af-2003的には
  - up to 15.4 W of DC power(minimum 44 VDC and 350 mA) to each device.Only 12.95 W is assured to be available at thepowered device (PD) as some power isdissipated in the cable.
- 距離上限は100m. cat5eと一緒．
- Approved: up to 12.95W (350mA) at 48V DC

– Approved in 2003 for PDs up to 12.95W (350mA) at 48V DC
– PoE requires handshake before applying power
– Power Sourcing Equipment (PSE) transmits power over 2 pairs (4 of 8 wires) on Cat5/5e/6 cable
– Connect/disconnect protocol for applying/disconnecting power
  - Detect device that needs power
  - Determines power needed
  - When to turn on power
  - Detect device disconnect (AC and DC disconnect)
  - When to remove power
– Physical layer mechanism for PSE’s to characterize power demands of individual PD’s and thus manage power delivered per port.

## IEEE 802.3btも存在．これはより大電力．(ref: http://www.ieee802.org/802_tutorials/2017-07/PracticalPoEtutorial-7-10-17.pdf)
- Type 3
  - Covers Classes 1-6
  - Classes 1-4 as before (i.e. IEEE 802.3-2015)
  - Class 5: 45W PSE, 40W PD
  - Class 6: 60W PSE, 51W PD
  - Minimum port voltage = 50V
- Type 4
  - Covers Classes 7 and 8
  - Class 7: 75W PSE, 62W PD
  - Class 8: 90W PSE, 71.3W PD
  - Minimum port voltage = 52V

## related commans on cisco ios
- show power inline
- (config-if)# power inline {auto|static|never} [priority {critical|high|low}]
- no lldp tlv-select power-management (Disables LLDP power negotiation(default: enabled))
- show power inline gigabitethernet 2/10 detail | begin LLDP

## references
- ref: https://www.ieee.li/pdf/viewgraphs/introduction_to_poe_802.3af_802.3at.pdf
- ref: https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst6500/ios/12-2SX/configuration/guide/book/power_over_ethernet.pdf