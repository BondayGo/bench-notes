# Modbus quick reference

Cheat sheet for the registers and errors we actually hit on the benches.

## Exception codes (FC03/FC04)

| Code | Name | Typical cause on our rigs |
| --- | --- | --- |
| 01 | Illegal function | drive firmware variant without the FC |
| 02 | Illegal data address | register map shifted — check drive doc revision |
| 03 | Illegal data value | writing out-of-range setpoint |
| 04 | Slave device failure | drive internal fault, check drive display |
| 06 | Slave device busy | drive booting or mid self-test, retry in 2 s |
| 11 | Gateway target failed | RS-485 converter lost the downstream device |

## Register maps we poll

### Drive (default slave 1, 19200 8N1)

| Register | Meaning | Scale |
| --- | --- | --- |
| 40001 | Heatsink temperature | 0.1 °C/count |
| 40003 | Motor speed | 1 rpm/count |
| 40010 | DC bus voltage | 0.1 V/count |
| 40021 | Status word | bitfield |

### Power meter (slave 2, same bus)

| Register | Meaning | Scale |
| --- | --- | --- |
| 40001 | Line voltage L1-N | 0.1 V/count |
| 40007 | Current L1 | 0.01 A/count |
| 40013 | Active power | 1 W/count |

## Frame layout (RTU, FC03 request)

```
| slave | 0x03 | addr hi | addr lo | count hi | count lo | crc lo | crc hi |
   1      1       1        1         1          1          1        1
```

CRC is sent low byte first — I got this wrong once and lost an afternoon,
hence the note.
