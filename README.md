# Gambas3 IoT Library

Gambas3 can be used to build applications for [IoT](https://en.wikipedia.org/wiki/Internet_of_things) devices and scenarios. IoT applications typically interact with sensors, displays and input devices that require the use of [GPIO pins](https://en.wikipedia.org/wiki/General-purpose_input/output), serial ports or similar hardware.

> ⚠️ This repository is still in experimental stage and all APIs are subject to changes.

# Hardware Requirements

- Any device that runs Linux >= v4.17

## Supported Peripherals

- `GPIO` (trough Linux [libgpiod](https://packages.debian.org/bookworm/libgpiod2))

- `I2C` (trough Linux [libi2c](https://packages.debian.org/bookworm/libi2c0))

- <mark>**TODO**</mark>: Add support to `SPI`

- **<mark>TODO</mark>**: Add support to `iio`

# Software Requirements

- Gambas3 >= v3.15

- [libgpiod](https://packages.debian.org/source/sid/libgpiod)

- [libi2c](https://packages.debian.org/bookworm/libi2c0)

# How To Install

Download the [latest](https://github.com/microhobby/gambas-iot/releases) released [gambas-iot.gambas](https://github.com/microhobby/gambas-iot/releases/download/v0.0.7/gambas-iot.gambas) executable file and execute it:

```bash
./gambas-iot.gambas
```

This will install the library on `/home/$USER/.local/share/gambas/lib/torizon/`

> ⚠️ If you are going to remotely deploy the application, remember to also install the library on the target, or copy the `gambas-iot.gambas` file to the same path as your application executable.

# Using GPIO Example

The "Hello World" of the embedded systems is blink a LED:

```vbnet
' Gambas module file

Public blinkTimer As Timer = New Timer As "Blink"
Public pinOutput As Pin = New Pin(0, 23)
Public pinInput As Pin = New Pin(0, 24) As "PinInput"

Public Sub Main()

    Print "Hello world"

    pinOutput.AsOutput()
    blinkTimer.Delay = 500
    blinkTimer.Start()

    pinInput.AsInput()
    pinInput.StartMonitorForPinChangedEvent()

End

Public Sub Blink_Timer()

    pinOutput.Toggle()

End


Public Sub PinInput_PinValueChanged(val As Integer)

    Print "Changed " & val

End
```

# Using i2c Example

```vbnet
' Gambas module file

' max17043 datasheet:
' http://cdn.sparkfun.com/datasheets/Prototyping/MAX17043-MAX17044.pdf

Public SparkFunLiPoFuelGauge As I2c

Public Sub Main()

    Dim data As Integer

    Print "Hello world"

    ' max17043 use 0x36 as address
    SparkFunLiPoFuelGauge = New I2c(1, &h36&)
    ' read max17043 register 0x08 to get chip production version
    data = SparkFunLiPoFuelGauge.ReadWord(&h08&)
    Print "MAX17040 version 0x" & Hex(data)

    ' write 0x5400 to max17043 register 0xFE to power-on reset the chip
    SparkFunLiPoFuelGauge.WriteWord(&HFE&, &h5400&)

    ' max17043 datasheet says that we need wait 500ms to get a voltage avg
    Sleep 0.5

    ' read max17043 register 0x02 to get battery VCELL
    data = SparkFunLiPoFuelGauge.ReadWord(&h02&)
    Print "VCELL :: " & (Shr(data, 4) / 1000) & "V"

End
```
