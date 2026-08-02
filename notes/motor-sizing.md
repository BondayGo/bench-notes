# Motor sizing rules of thumb (bench rigs)

Back-of-envelope checks before opening the selection catalog. Real picks
still come from the vendor software; this just catches the "that can never
work" cases early.

## Torque

```
T_load = F * r            (linear load through a belt/rack)
T_wheel = m * g * r * mu  (traction, mu ~ 0.3..0.5 for cart on rails)
```

Continuous torque needed from the motor:

```
T_cont = T_load / (gear_ratio * efficiency)   efficiency: belt ~0.9, worm ~0.6
```

Peak torque (acceleration):

```
T_peak = J_total * alpha + T_load
J_total = J_motor + J_load / gear_ratio^2
```

## Inertia ratio

Keep `J_load / J_motor` (referred through the gearbox, so divided by
ratio²) under ~10 for positioning rigs, under ~5 if you want tight settling.
Over 20 the rig tunes "crunchy" no matter how long you spend on gains.

## Speed

```
n_max [rpm] = v_max [m/s] * 60 / (2 * pi * r [m])
```

Check it against the motor's rated speed, not its max speed — running at
max speed all day cooks bearings on the small frames.

## Quick sanity numbers for our benches

| Unit class | Typical cont. torque | Typical bus voltage |
| --- | --- | --- |
| Small NEMA 23 class | 0.5–1.5 N·m | 48 V |
| Mid NEMA 34 class | 2–4 N·m | 48–72 V |
| Servo 400 W class | 1.3 N·m cont | 220 V (careful) |

## Thermal

Heatsink temperature register (40001) above ~70 °C cont. means the duty
cycle assumption was wrong, not that the fan is broken. Check both before
swapping parts.
