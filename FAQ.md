# Frequently Asked Questions

## How can I perform an OTA firmware update on BLE devices over `pouch`?

For earliest enrollees in the private access program, only device-to-cloud
capabilities are enabled The program will be expanded with support for
cloud-to-device communication, and all services will be supported for public
access.

Read more about Golioth’s device management services
[here](https://docs.golioth.io/device-management).

## How is communication secured between gateways and Golioth?

Communication between gateways and Golioth is [secured in transit using
TLS/DTLS](https://blog.golioth.io/life-of-a-coap-message/#establishing-a-secure-channel)
in the same manner as any other device that connects to the platform.

## How is communication secured between devices and gateways?

Device to gateway communication may be secured using native transport-specific
capabilities. For example, BLE devices may leverage [LE secure
connections](https://www.bluetooth.com/blog/bluetooth-pairing-part-4/). This is
outside of the scope of `pouch`.

In the private access example, no transport level security is employed between
the device and the gateway. This is not recommended in production scenarios
unless end-to-end encryption is in use (see next question).

## How is communication secured between BLE devices and Golioth?

In many cases it is undesirable for the gateway to have visibility into the data
it is proxying, even if there is a secure connection between the device and
gateway, and between the gateway and Golioth. Though not enabled in the private
access example, `pouch` supports end-to-end encryption between devices and the
Golioth platform using public key cryptography with an offline key agreement
scheme.

Private access enrollees will be notified in the future when the platform
functionality required to support end-to-end encryption is enabled in their
project.

## How much overhead does `pouch` introduce to data transmission?

Using `pouch` can introduce overhead at the transport (e.g. BLE) and protocol
(i.e. the messages themselves) level. The amount of overhead depends on the type
of data being sent, the block size used, and the encryption parameters employed.
Because encryption is not employed and block sizes have not been tuned in the
private access example, it does not serve as a representative sample for `pouch`
overhead.

Minimizing overhead is a primary objective in the `pouch` specification, and we
are developing it with that goal in mind. There are a handful of simple
optimizations that can be introduced, and employing compression can result in
significant payload size reduction in production scenarios.

Importantly, regardless of the overhead introduced by `pouch` on the wire, it
does *not* increase costs when using
[Pipelines](https://docs.golioth.io/data-routing) on Golioth as pouches are
“unpacked” prior to transformation and delivery to external destinations.

If you are interested in further discussion of overhead and performance, please
reach out to us.

## What if my BLE devices cannot run custom firmware (e.g. beacons)?

Devices that cannot run custom firmware cannot leverage `pouch`, but the Golioth
platform will support these scenarios via *impersonation*, a new feature that
allows a gateway send data on behalf of another device. You can read more about
the difference between proxying and impersonation
[here](https://blog.golioth.io/the-taxonomy-of-connected-device-networks/#impersonation-and-proxying).

If you think impersonation may be required for your use case, reach out to us
for access.

## What BLE device hardware is currently supported?

While using the [nRF52840
DK](https://www.nordicsemi.com/Products/Development-hardware/nRF52840-DK) is
recommended for the private access program, any Zephyr-supported device with BLE
capabilities should be able to leverage `pouch` to communicate via gateways
running the provided firmware images.

## What gateway hardware is currently supported? What about mobile applications?

Pre-built gateway firmware images are provided for the [nRF9160
DK](https://www.nordicsemi.com/Products/Development-hardware/nRF9160-DK) and the
[Thingy:91
X](https://www.nordicsemi.com/Products/Development-hardware/Nordic-Thingy-91-X)
as part of the private access program. Because `pouch` is gateway agnostic,
development kits can be used for early prototyping before moving to a custom or
off-the-shelf gateway for production.

One of the most common forms of a gateway is a mobile application running on a
user’s smartphone. `pouch` is an excellent choice for delivering data to the
cloud or delivering OTA updates via a mobile device, and we have introduced
*identity federation* to allow for mobile applications to authenticate to
Golioth using external user credentials. You can read more about identity
federation
[here](https://blog.golioth.io/the-taxonomy-of-connected-device-networks/#electric-scooter-and-mobile-application).

If interested in developing a mobile application that supports proxying data
between BLE devices and the cloud, reach out to us for more information.
