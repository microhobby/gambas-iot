# Gambas3 IoT Library

Gambas3 can be used to build applications for [IoT](https://en.wikipedia.org/wiki/Internet_of_things) devices and scenarios. IoT applications typically interact with sensors, displays and input devices that require the use of [GPIO pins](https://en.wikipedia.org/wiki/General-purpose_input/output), serial ports or similar hardware.



> ⚠️ This repository is still in experimental stage and all APIs are subject to changes.



# Hardware Requirements

- Any device that runs Linux >= v4.17



# Software Requirements

- Gambas3 >= v3.15

- [libgpiod](https://packages.debian.org/source/sid/libgpiod)



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
