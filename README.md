# pico-serprog-iCE40HX1K-EVB

This is a basic flashrom/serprog compatible SPI flash reader/writer for the Raspberry Pi Pico.

This version is aimed for programming Olimex iCE40HX1K-EVB via the IDC10 connector.

It does not require a custom version of flashrom, just drag the compiled uf2 onto your Pico and you're ready to go.

The default pin-out is:

| Pico Pin | Pico GPIO | iCE40HX1K Pin | Function |
|----------|-----------|---------------|----------|
| 1        | 0         | 6             | CRESET   |
| 2        | 1         | 10            | SS_B     |
| 3        | -         | 2             | GND      |
| 4        | 2         | 9             | SCK      |
| 5        | 3         | 7             | SDI      |
| 6        | 4         | 8             | SDO      |
| 36       | -         | 1             | 3V3      |

 
## Usage

Dump a flashchip:

```
flashrom -p serprog:dev=/dev/ttyACM0:115200,spispeed=12M -r foo.bin
```

Add a padding (as generated bitstreams are smaller than size of the flash chip) and write a bitstream:

```
dd if=/dev/zero bs=2M count=1 | tr '\0' '\377' > foo_padded.bin
dd if=foo.bin conv=notrunc of=foo_padded.bin
flashrom -p serprog:dev=/dev/ttyACM0:115200,spispeed=12M -w foo_padded.bin
```

## License

The project is based on the spi_flash example by Raspberry Pi (Trading) Ltd. which is licensed under BSD-3-Clause.

As a lot of the code itself was heavily inspired/influenced by `stm32-vserprog` this code is licensed under GPLv3.
