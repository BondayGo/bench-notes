# Serial / RS-485 debugging traps

Things that have cost me hours, written down so they cost me once.

## Wiring

- A/B labels are NOT standardized. Vendor A is sometimes the other vendor's
  B. If the bus is silent, swap the pair once before trusting anything else.
- Termination: 120 Ω at both physical ends only. A switch in the middle
  murders the signal at higher baud rates.
- Ground reference matters even on a "differential" pair. Our benches tie
  signal GND at the drive end; leaving it floating gave us CRC errors only
  when the spindle motor ran — took a week to find.

## Ports

- Windows COM numbers shuffle after replugging into a different USB port.
  Label the dongle and always use the same physical port (bench = COM3).
- Cheap USB-RS485 dongles echo what you transmit. If your reply parser sees
  your own request, check whether the dongle has auto-direction control;
  the ones without need RTS toggling, which pyserial does not do for you.
- `port.read(n)` returns early with whatever arrived. Always check length,
  never assume you got n bytes.

## Timing

- RTU silence between frames: 3.5 character times. At 19200 that is about
  2 ms — flush the input buffer before each request or you parse leftover
  bytes from the previous reply.
- After a drive power cycle, give it 5 s before the first poll. Requests
  during boot return exception 06 or nothing at all.

## Tools

- A plain multimeter across A-B should read ~0.6–1.2 V difference on an
  idle terminated bus. 0 V or rail-to-rail means wiring, not software.
- When in doubt, poll the power meter (slave 2). If it answers and the
  drive does not, the bus is fine and the drive is sulking.
