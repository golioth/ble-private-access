# Troubleshoot

Having trouble connecting your BLE devices to Golioth? This document may help
you identify and correct any issues.

## Accessing Thingy:91 X Logs and Shell

The Thingy:91 X typically emits logs from each core over serial using
the UART-USB [Connectivity
Bridge](https://github.com/nrfconnect/sdk-nrf/tree/main/applications/connectivity_bridge)
firmware on the nRF5340 application core (`nrf5430_cpuapp`). However, the UART is
re-allocated for communication between the nRF5340 network core
(`nrf5340_cpunet`) and the nRF9151
in the gateway firmware, so no serial ports appear when the Thingy:91 X is connected to the
host machine via USB. Instead, each of the cores is configured to use the [RTT
console](https://github.com/zephyrproject-rtos/zephyr/tree/main/snippets/rtt-console).

Logs and shell access for each core on the Thingy:91 X can be obtained using
`JLinkRTTViewer`. Upon connecting the J-Link, open the viewer, select `Connect`,
and configure the target as `NRF9151_XXCA`, `NRF5340_XXAA_NET`, or
`NRF5340_XXAA_APP` to access logs and the shell on each core.

> [!NOTE]
> The SWD selection switch must be set to `nRF91` for connecting to the
> `nRF9151` and `nRF53` when connecting to the `nRF5340` network and application
> cores.

## BLE Device Session Established but No Data Visible

If your BLE device is successfully connecting to Golioth, but there is no data
from the device visible in [LightDB
Stream](https://docs.golioth.io/application-services/lightdb-stream/), make sure
that you have created and enabled a
[Pipeline](https://docs.golioth.io/data-routing) that accepts JSON data.

## Command Fails when Programming Device

`nrfutil` commands may fail if the target is in a bad state. The problem can
typically be resolved by issuing an `nrfutil device recover` command then
attempting to program again.
