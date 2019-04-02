# Reset Configuration Word for the SMARC-sAL28

This repository contains the reset configuration words for the different
SMARC PCIe lane use cases.

## What is the RCW doing?

The RCW is responsible for setting up the correct functions of the multi
purpose pins, the SerDes procotol, loading the initial bootloader and
transfering control to it.

Because the SerDes protocol cannot be changed after booting we deliver
different binary versions which can be transferred to the I2C EEPROM, where
it is read by the processor. There is always a hard-coded RCW for a
rescue system in the SPI flash of the module.

In particular, the RCW
* loads the actual reset configuration word,
* sets up the FlexSPI lookup table to use fast read dual I/O SPI mode,
* sets the SPI clock,
* applies workarounds for chip errata,
* enables the GPIO input buffer on all GPIO used as inputs (there are not
  user GPIOs connected to the SoC. All SMARC GPIOs are controlled by a CPLD
  on the module),
* sets the boot location to the internal SRAM,
* figures out the boot source by looking at the `rcw_src_cfg[3:0]` pins and
  copies the bootloader payload from the corresponding location.

## Building your own RCW

Although not supported by Kontron, you could build your own RCW by using
the utilities and LS1028A templates available in the [upstream
repository][1].

## Bootloader payload locations

| RCW source | Bootloader location   | Notes         |
| ---------- | --------------------- | ------------- |
| I2C        | SPI flash @`21_0000h` | normal boot   |
| SPI        | SPI flash @`1_0000h`  | failsafe boot |
| SDHD card  | SDHC card @1MiB       | production    |

## Naming conventions

The filename `sl28-<variant>-<PCIe lane mapping>[-<special_option>].bin`.

Examples:

* `sl28-3-22_q.bin` Single PHY module with PCIe x2
* `sl28-4-11_q.bin` Dual PHY module with 2x PCIe x1

## Available Variants

| Variant | Description         |
| ------- | ------------------- |
| `1`     | reserved            |
| `2`     | Dual TSN port       |
| `3`     | Single PHY w/ audio |
| `4`     | Dual PHY w/o audio  |

## Available SMARC PCIe lane mappings

The naming convention is `<PCIeA><PCIeB><PCIeC><PCIeD>`. Where `<PCIeN>` is
one of the following protocols:

| Character | Description       |
| --------- | ----------------- |
| `1`       | PCIe x1 lane      |
| `2`       | PCIe x2 lane      |
| `s`       | SATA lane         |
| `g`       | SGMII lane        |
| `q`       | QSGMII lane       |
| `_`       | lane not avalable |

All variants only supports different SerDes protocols on SMARC PCIe A/B
lanes. Please note that the protocols in (brackets) are optional features
not specified in the SMARC specification. Additionally, variant 3 and 4
always have QSGMII on the PCIe D lane.

Due to a limitation of the LS1028A SoC, PCIe Gen3 cannot be used
simultaneously with SATA. Therefore, if a RCW with SATA is programmed, PCIe
will only negotiate to to PCIe Gen1 or Gen2.

### Variant 2: Dual TSN port module

| Suffix | Description        |
| ------ | ------------------ |
| `11__` | PCIe x1 / PCIe x1  |
| `22__` | PCIe x2            |
| `1s__` | PCIe x1¹ / (SATA)  |

### Variant 3: Single PHY module

| Suffix | Description        |
| ------ | ------------------ |
| `11_q` | PCIe x1 / PCIe x1  |
| `22_q` | PCIe x2            |
| `1s_q` | PCIe x1¹ / (SATA)  |

### Variant 4: Dual PHY module

| Suffix | Description        |
| ------ | ------------------ |
| `11_q` | PCIe x1 / PCIe x1  |
| `22_q` | PCIe x2            |
| `1s_q` | PCIe x1¹ / (SATA)  |

¹Only PCIe Gen1/Gen2

[1]: https://source.codeaurora.org/external/qoriq/qoriq-components/rcw/
