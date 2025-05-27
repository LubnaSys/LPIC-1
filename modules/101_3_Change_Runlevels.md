Change runlevels / boot targets and shutdown or reboot system
---
## First check init version
`ps -p 1 -o comm=`
If it outputs systemd, you are using systemd.
If it outputs init (and you're on a modern system), it's still likely systemd, but init is a symlink to it.
If it outputs upstart, you're using Upstart.
If it outputs runit, you're using Runit
---
## 📋 Systemd: Runlevels / Boot Targets and System Control

| Traditional Runlevel | Systemd Target   | Command to Switch Temporarily           | Command to Set as Default (Boot)           |
| :------------------- | :--------------- | :-------------------------------------- | :----------------------------------------- |
| 0 - Halt             | `poweroff.target`  | `systemctl isolate poweroff.target`     | `systemctl set-default poweroff.target`    |
| 1 - Single-user mode | `rescue.target`    | `systemctl isolate rescue.target`       | `systemctl set-default rescue.target`      |
| 3 - Multi-user (no GUI) | `multi-user.target` | `systemctl isolate multi-user.target`   | `systemctl set-default multi-user.target`  |
| 5 - Multi-user + GUI | `graphical.target` | `systemctl isolate graphical.target`    | `systemctl set-default graphical.target`   |
| 6 - Reboot           | `reboot.target`    | `systemctl isolate reboot.target`       | `systemctl set-default reboot.target` (⚠️ not common) |

---
## ⚙️ Shutdown / Reboot / Poweroff Commands

| Action              | Command                | Alternative                        |
| :------------------ | :--------------------- | :--------------------------------- |
| Shutdown immediately | `systemctl poweroff`   | `shutdown now`                     |
| Reboot immediately  | `systemctl reboot`     | `shutdown -r now`, `reboot`        |
| Halt system         | `systemctl halt`       | `halt`                             |
| Schedule shutdown   | `shutdown +5`          |                                    |
| Cancel shutdown     | `shutdown -c`          |                                    |
