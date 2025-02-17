# ACPI_CALL - A Linux Kernel

A simple kernel module that enables you to call ACPI methods by writing the
method name followed by arguments to `/proc/acpi/call`.

acpi_call is used from the terminal, if you're afraid or simply want a graphical experience, [see the dedicated section below](#graphic-interface).

## ⚠️ Caution ! ⚠️

This module is to be considered a proof-of-concept and has been superseeded by
projects like [bbswitch](https://github.com/Bumblebee-Project/bbswitch). It
allows you to tamper with your system and should be used with caution.

The commands work on the [author](https://github.com/mkottman)'s ASUS K52J notebook, but may not work for you.
See [linux-hybrid-graphics.blogspot.com](http://linux-hybrid-graphics.blogspot.com/) or try running the provided scripts (e.g. `examples/turn_off_gpu.sh`).

It SHOULD be ok to test all of the methods, until you see a drop in battery
drain rate (`grep rate /proc/acpi/battery/BAT0/state`), however it comes
with NO WARRANTY - it may hang your computer/laptop, fail to work, etc.

## Installation

## Usage

You can call acpi_call this way:

```sh
echo '<call>' | sudo tee /proc/acpi/call
```

You can then retrieve the result of the call by checking your dmesg or:

```sh
sudo cat /proc/acpi/call
```

An example to turn off discrete graphics card in a dual graphics environment
(like NVIDIA Optimus):

```sh
# turn off discrete graphics card
echo '\_SB.PCI0.PEG1.GFX0.DOFF' > /proc/acpi/call
# turn it back on
echo '\_SB.PCI0.PEG1.GFX0.DON' > /proc/acpi/call
```

You can pass parameters to `acpi_call` by writing them after the method,
separated by single space. Currently, you can pass the following parameter
types:

- ACPI_INTEGER - by writing NNN or 0xNNN, where NNN is an integer/hex
- ACPI_STRING - by enclosing the string in quotes: "hello, world"
- ACPI_BUFFER - by writing bXXXX, where XXXX is a hex string without spaces,
                or by writing { b1, b2, b3, b4 }, where b1-4 are integers

The status after a call can be read back from `/proc/acpi/call`:

- 'not called' - nothing to report
- 'Error: <description>' - the call failed
- '0xNN' - the call succeeded, and returned an integer
- '"..."' - the call succeeded, and returned a string
- '{0xNN, ...}' - the call succeeded, and returned a buffer
- '[...]' - the call succeeded, and returned a package which may contain the
   above types (integer, string and buffer) and other package types


## Graphical interfaces

If you found this too difficult for you or simply want a graphical experience give a try to these programs provided by [Marco Dalla Libera](https://github.com/marcoDallas/):

- [acpi_call_GUI (Ubuntu and other debian-based distributions)](http://marcodallas.github.io/acpi_call_GUI/)
- [acpi_call_GUI_Fedora (Fedora version)](https://github.com/marcoDallas/acpi_call_GUI_Fedora)

## Issues, Pull Requests and Contributions

If you want to open an issue, propose a fix or just want to improve this kernel module overall, you have two choices:

- Open an [Issue](https://github.com/alexis-opolka/acpi_call_made_easy/issues) or [Pull Request](https://github.com/alexis-opolka/acpi_call_made_easy/pulls) on this repository regarding changes concerning modifications made on this fork
- Open an [Issue](https://github.com/mkottman/acpi_call/issues) or [Pull Request](https://github.com/mkottman/acpi_call/pulls) on the [original repository](https://github.com/mkottman/acpi_call) regarding problems with the original code.

  Please mention me (i.e. `@alexis-opolka`) if you open an issue on the original repository and want a fix to appear on my fork.
  I will greatly appreciate it. ^-^

---

## Copyright

Copyright &copy; 2010: Michal Kottman  
Copyright &copy; [Biji](https://github.com/biji/)  
Copyright &copy; 2023 Alexis Opolka

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
