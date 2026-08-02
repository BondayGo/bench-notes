# Daily checklist (before a run)

Five minutes that save an afternoon.

## Power

- [ ] Bench E-stop released, guard closed
- [ ] Main breaker on; 24 V supply LED green (bench A has a flaky terminal,
      wiggle it once)
- [ ] Drive DC bus discharged (wait the sticker time after power-off before
      touching anything)

## Mechanics

- [ ] Coupling guard bolts tight (torque marked on the guard)
- [ ] Motor mounted on the correct riser block for the unit under test
- [ ] No loose tools on the table; torque wrench back on the wall

## Signal

- [ ] RS-485 dongle on COM3, termination switch ON at both ends
- [ ] Thermocouple / encoder connectors seated (they look plugged in when
      they are half out — push until the click)
- [ ] Start `modbus-trend` logging BEFORE the first spin, not after

## After the run

- [ ] Save the CSV into `reports/<date>-<unit>/`
- [ ] Note anything abnormal in the run sheet, even if it "went away"
- [ ] Power down in order: drive first, then control PC, then bench supply
