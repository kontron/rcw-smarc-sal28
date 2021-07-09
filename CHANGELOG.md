# Changelog

## unrel
 - Add special RCW `sl28-4-22_q-s.bin`

## v11
 - Enable SATA RX lane swap
 - Add workaround for A-010554 (Improve SATA hard drive detection)
 - Add workaround for A-009531 (Wrong IDO bit value for PCIe completion
   packets)

## v10
 - Add network variant 1 RCWs

## v9
 - Fix I2C read access.

## v8
 - Add U-Boot script to ease the RCW installation.
 - Fix FlexSPI LUT access.

## v7
 - Fix SAI4 and SAI5 multiplexer settings.

## v6
 - Embed the RCW filename in the image.
 - Increase SPL size to 20000h. This will allow bigger U-Boot SPL binaries.

## v5
 - Add new boot sources: eMMC, FlexSPI CS#1 and DualSPI CS#0.

## v4
 - Add PEX Gen3 errata workarounds (A-010477, A-008851)
 - Fix hardware flags in GPINFO.

## v3
 - Unify boot image and boot method for all boot sources.

This needs a new bootloader image!

## v2
 - encode variant and PHY options in GPINFO.
 - enable GPIO input buffer.
 - change PLL2 to 125MHz clock input. Earlier versions using the PLL2 kept
   rebooting.

## v1
 - Initial release.
