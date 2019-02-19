This repository contains the reset configuration words for the different
SMARC PCIe lane use cases.

# Naming conventions

The filename `sl28-<variant>-<PCIe lane mapping>[-<special_option>].bin`.

Examples:

* `sl28-3-22_q.bin` Single PHY module with PCIe x2
* `sl28-4-11_q.bin` Dual PHY module with 2x PCIe x1

# Available Variants

| Variant | Description         |
| ------- | ------------------- |
| `1`     | reserved            |
| `2`     | Dual TSN port       |
| `3`     | Single PHY w/ audio |
| `4`     | Dual PHY w/o audio  |

# Available SMARC PCIe lane mappings

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

## Variant 2: Dual TSN port module


| Suffix | Description       |
| ------ | ----------------- |
| `11__` | PCIe x1 / PCIe x1 |
| `22__` | PCIe x2           |
| `1s__` | PCIe x1 / (SATA)  |

## Variant 3: Single PHY module

| Suffix | Description       |
| ------ | ----------------- |
| `11_q` | PCIe x1 / PCIe x1 |
| `22_q` | PCIe x2           |
| `1s_q` | PCIe x1 / (SATA)  |

## Variant 4: Dual PHY module

| Suffix | Description       |
| ------ | ----------------- |
| `11_q` | PCIe x1 / PCIe x1 |
| `22_q` | PCIe x2           |
| `1s_q` | PCIe x1 / (SATA)  |
