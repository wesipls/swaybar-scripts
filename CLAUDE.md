# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A collection of small C programs and shell scripts that output system stats for swaybar (sway's status bar), following the [swaybar-protocol](https://swaybar.github.io/swaybar-protocol/). `status.sh` is the entry point that orchestrates all components and emits the JSON stream swaybar expects.

The scripts are deployed to `~/.config/sway/statbar/` — `status.sh` calls binaries and scripts from that hardcoded path.

## Build

```bash
make          # build all C binaries
make memory   # build a single binary
```

Compiler: `gcc`. The `gpu` target links against `-lrocm_smi64` (AMD ROCm SMI library).

## Hardware-specific values

Several files hardcode values that may need adjusting per machine:

- **`network_in_out.c`**: Interface hardcoded to `enp5s0` (paths under `/sys/class/net/enp5s0/statistics/`)
- **`status.sh`**: CPU temp uses `sensors | awk '/Tccd1/...'` — `hwmon` sensor names can change between reboots
- **`disk_io.sh`**, **`disk_temp.sh`**: May assume a specific disk device

## Architecture

Each component is self-contained and independently executable. They read from `/proc`, `/sys`, or external tools, and print a single line to stdout.

- **C programs** (`memory.c`, `swap.c`, `cpu.c`, `gpu.c`, `disk.c`, `network_in_out.c`): read kernel interfaces directly; use `/tmp` files to cache previous values for delta calculations (network I/O, disk I/O)
- **Shell scripts** (`weather.sh`, `updates.sh`, `disk_io.sh`, `disk_temp.sh`): wrap external tools (`curl`/wttr.in, `apt`, `smartctl`)
- **`status.sh`**: runs all components every 5 seconds, wraps output in swaybar JSON blocks with `min_width` and `align` fields, and emits the infinite JSON array the protocol requires

The 5-second polling interval is assumed by `network_in_out.c` (`time_diff = 5`) and `disk_io.sh`. Change both if adjusting the sleep in `status.sh`.

Weather output is cached in `/tmp/weather` and refreshed every 30 minutes.
