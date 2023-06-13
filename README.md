# jetson-l4t

A rootful container that is setup to run `boardctl` commands or flash a jetson
using the l4t environment. It requires root access to install and run.
The scripts expect `podman` to be the container tool used.

## Installing a jetson-l4t container

A script, `jetson-l4t`, will be installed to `$HOME/.local/bin`. This is the
script used to flash, run board control commands and drop into a shell.

Firmware releases supported:

- 35.3.1
- 35.2.1

```
./setup.sh -i -f <firmware release> -t <container version tag, i.e. 35.3.1>
```

### Installing a jetson-l4t container with a BSP

Download the BSP you want to overlay ontop of the `Linux_for_Tegra` directory
and specify the path to the tarfile using `-p /path/to/bsp.tarfile`. If the
installation for the bsp requires something other then `apply_binaries.sh`
being run, you need to provide a shell script that runs those commands and
specify it using `-s /path/to/script.sh`. You can use the `files/setup.sh`
script as a starting point.

```
./setup.sh -i -f <base firmware release> -t <container version tag> -p /path/to/bsp.tarfile -s /path/to/bsp_setup.sh
```

### Notes

The `-i` argument performs a full install of the jetson-l4t container and user
runtime script. This only needs to be run once, for additional containers, just
use the `-b` argument to build a new contianer.

If you do not want to include the Sample filesystem in the `Linux_for_Tegra`
directory, add the `-n` argument to your install or build setup commands.

## Flashing a Jetson

Supported Jetson boards:

- agxorin
- igxorin
- orinnx
- xaviernx

The Jetson board config can be found in the `Linux_for_Tegra` directory, i.e. jetson-agx-orin-devkit.conf.
Drop the `.conf` file extension when specifying which config flash should use.

```
jetson-l4t -t <container version tag> -j <jetson board> -c flash -b <jetson board config>
```

## Running a board control command

Board control commands:

- power_on
- power_off
- recovery
- reset
- status

The board control commands are currently only supported for the AGX Orin.

```
jetson-l4t -t <container version tag> -j <jetson board> -c <board control command>
```

## Getting a shell for running manual commands

This will drop you into the specified jetson-l4t container at the `Linux_for_Tegra` directory.

```
jetson-l4t -t <container version tag> -j <jetson board> -c shell
```
