Warning: the bitaxeHex is unfinished. there may be hardware and firmware bugs. Please see The bitaxeHex channel on the [OSMU Discord](https://discord.gg/osmu) for the latest status

> Closed Source is Antithetical to Bitcoin

# Presenting: The bitaxeHex
bitaxeHex is a follow on to the [bitaxe](https://github.com/skot/bitaxe) that incorporates six BM1366 ASICs from the Antminer S19XP

![bitaxeHex render](doc/hex_render.jpg)

## Goals
- **Standalone**: can mine directly to your pool over WiFi. No External computer needed.
- **Embedded**: low cost, low maintenance, high availability, high reliability, low power.
- **ASIC**: based on the very efficient BM1366 from Bitmain.
- **Versatile**: solo/pool mining, autotune power/heat/efficiency.
- **Open Source**: All design files are provided.

## Features
- **ESP32-S3-WROOM-1** wifi microcontroller on board
- **TI TPS546D24ARVFR** buck regulator steps down the 12V input to power the chain of BM1366
- **TMP1075** measures inlet and outlet PCB temperature.
- **Microchip EMC2302** Controls dual fans
- Header for optional status LCD

## BM1366
- The BM1366 is a undocumented SHA256 mining ASIC from Bitmain. It's mostly used in the Antminer S19XP and S19K Pro
- Bitmain claims the BM1366 has 21.5 W/TH efficiency
- The BM1366 is available (new) for around $15 each.

## Current Status
- v304 is the latest revision but it has a known problem with the Vcore regulator.  There is a workaround, see the bottom of this readme.
- Power draw is around 50W @12V.
- ESP32 miner firmware will configure the power supply to run at the proper voltage.
- This is an _advanced_ build! If you don't have experience building boards, you should probably go build a single ASIC bitaxe first to get the technique down.

## Notes
- Beware that overclocking doesn't produce a higher hash rate on this board, it only causes more heat generation.
- If you decide to build a pre-release board, be sure to order 2 oz. copper on both the internal and external layers.
- The recommended heatsink is not anodized and very conductive on all surfaces. This _should_ be OK as the ASIC tops are not known to be conductive, but this needs some more investigation.

## Revision List
- v304 has a few new minor features, but it has a problem with the Vcore output capacitance.
- v303 has some improvements in the layout and the 33R inline resistors have been removed.
- V302 is the current working version of this board.  If you want to build this, pull down this git tag.
- V301 does not work, the power supply maintains voltage, but overheats in a drastic way
- V300 does not work, the power supply cannot maintain voltage when a load is applied

## Hardware
- [BM1366 from NBTC on AliExpress](https://www.aliexpress.us/item/3256803471845503.html). Both the `AL` and `AG` variants have been known to work.
- [Heatsink](https://www.aliexpress.us/item/3256805608902122.html) 90mm long variant. This will need to have threaded M3 mounting holes added. See the KiCad board file for the pattern
- [Fans](https://www.amazon.com/Noctua-NF-A8-PWM-Premium-Quiet/dp/B00NEMG62M) At least one 80x80mm 12V 4-pin fan. Like the Noctua NF-A8 PWM. Possibly two.
- [Enclosure](https://www.aliexpress.us/item/3256806064761702.html) (several sellers) 130mm long variant. The bitaxeHex needs to be run inside an enclosure to force air through the heatsink and effectively cool the BM1366s
- The TPS546 can potentially benefit from a [small self-adhesive heatsink](https://www.amazon.com/gp/product/B093V6RK58/) on the top of the package.
- All of the parts on the board are listed in the KiCad BOM

## Software
- [ESP-Miner-multichip](https://github.com/bitaxeorg/esp-miner-multichip) a fork of esp-miner to support multiple ASICs like on the bitaxeHex.

## Power Supply Requirements
- bitaxeHex takes 12V DC input via screw terminals. Power supply should be capable of 100W
  - This [120W Brick](https://www.amazon.com/gp/product/B07PWZQ33N) technically works, but gets a little warmer than it should. You'll need to change the end.
  - 12V 15A 110V AliExpress [Bare PSU](https://www.aliexpress.us/item/3256805439916551.html)

## Building
- Check out [building.md](building.md) for PCB ordering tips
- Check out [assembly.md](assembly.md) for assembly tips

## V304 Modification
The Vcore regulator output doesn't have enough bulk capacitance, and the regulator won't run in a stable mode.
In order to fix this, you can hack on a couple 180 uF Aluminium Polymer electrolytic capacitors.  See this diagram for the schematic and part numbers.  The specified parts have wire leads, and you can solder one on each side of the board across the existing ceramic output caps.

![v304 modification](doc/v304-fix.png)
