# Troubleshoot an Unstable Switch Link

This runbook was derived from the ASUS AP uplink investigation on the EX2200.

## Symptoms

- Carrier-transition count increases
- Link renegotiates or downshifts
- Clients lose connectivity intermittently
- CRC counters may remain zero despite physical flaps

## Baseline

Capture:

```text
show interfaces <interface> extensive
show ethernet-switching table interface <interface>
show spanning-tree interface <interface>
```

Record link speed, duplex, last flap, carrier transitions, CRC/framing errors, drops, VLAN membership, and learned MAC count.

## Isolation sequence

1. Confirm the intended VLAN and switch-port mode.
2. Replace or reseat the patch cable.
3. Clear interface statistics.
4. Move the unchanged endpoint and cable to a known-good switch port.
5. Monitor both the original and replacement port.
6. Compare new carrier transitions and error counters.
7. Change one variable at a time.

## Cable diagnostics

When supported and operationally safe:

```text
request diagnostics tdr start interface <interface>
show diagnostics tdr interface <interface>
```

Some platforms require the interface to be administratively disabled for accurate TDR results. Confirm platform behavior before testing.

## Interpretation

- Flaps follow endpoint/cable: investigate endpoint power, NIC, or cable.
- Flaps remain on original port: investigate switch PHY or port hardware.
- CRC/framing errors increase: investigate cabling, interference, or negotiation.
- Counters remain clean after a port move: continue monitoring before declaring long-term resolution.

## Completion criteria

- Expected speed and full duplex
- No new carrier transitions during the validation window
- No CRC, framing, or input errors
- Stable client connectivity
- Port description updated to the current endpoint
