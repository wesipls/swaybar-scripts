# statbar

Small C programs and shell scripts for swaybar.

> [!NOTE]
> These programs are provided as-is and may require modifications.
> If something does not work, there should be a comment at the top of the file explaining why.

---

## Usage

Use `status.sh` as your `status_command` in sway:

```bash
bar {
    status_command ~/.config/sway/statbar/status.sh
}
```

---

## Programs

### status.sh
Main script that runs all programs and formats output for swaybar.

---

### memory.c
Displays current RAM usage  
**Output:** `RAM: 8.2G/16G`

---

### swap.c
Displays current swap usage  
**Output:** `SWP: 2.5G/8G`

---

### cpu.c
Displays current CPU usage  
**Output:** `CPU: 15%`

---

### gpu.c
Displays GPU usage and temperature  
**Output:** `GPU: 15% +65°C`  
**Note:** Requires ROCm SMI and compatible AMD GPU

---

### network_in_out.c
Displays network throughput  
**Output:** `NET: ↓ 1.5M / ↑ 800K`  
**Note:** Interface is hardcoded, adjust if needed

---

### disk.c
Displays disk usage for `/`  
**Output:** `DISK: 45%`

---

### disk_io.sh
Displays disk read/write speed  
**Output:** `↑ 1.2M / ↓ 500K`  
**Note:** Assumes 5s interval

---

### disk_temp.sh
Displays disk temperature via `smartctl`  
**Output:** `+35°C`  
**Note:** Requires `smartmontools` and permissions for `smartctl`

---

### weather.sh
Displays weather using wttr.in  
**Output:** `☀️   0mm   ↘5km/h   +15°C   (+13°C)`  
**Note:** Cached in `/tmp/weather`, refreshes every 30 minutes

---

### updates.sh
Displays number of available package updates  
**Output:** `2 upgrades available`  
**Note:** Debian/apt-based systems only

---

## Notes

- Most programs assume a 5 second refresh interval
- Some values are cached in `/tmp` for performance
- Hardware-specific values (network interface, disk, GPU) may need adjusting
