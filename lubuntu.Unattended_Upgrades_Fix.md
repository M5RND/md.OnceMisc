# ISSUE:

`Citing for cache lock: Could not get lock /var/lib/dpkg/lock. It is held by process 34145 (dpkg)... 8s`

```
$ ps aux|grep unattend
root        1335  0.0  0.0 118060  3224 ?        Ssl  May06   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
root       34126  1.0  4.1 494116 154220 ?       S    10:24   0:00 /usr/bin/python3 /usr/bin/unattended-upgrade
```

# TIP: Stop Unattended Upgrades Permanently

You have unattended upgrades running and holding the dpkg lock. Here's how to disable it permanently:

## Immediate fix (stop the process now):
```bash
sudo systemctl stop unattended-upgrades
sudo systemctl stop apt-daily.service
sudo systemctl stop apt-daily-upgrade.service
```

## Permanent fix (prevent it from running again):
```bash
sudo systemctl disable unattended-upgrades
sudo systemctl disable apt-daily.service
sudo systemctl disable apt-daily-upgrade.service
sudo systemctl mask unattended-upgrades
```

## Verify it's disabled:
```bash
sudo systemctl status unattended-upgrades
```

## Optional: Remove the package entirely
If you never want unattended upgrades:
```bash
sudo apt remove unattended-upgrades
```

## Why this happens
- Unattended upgrades runs on a schedule (usually daily)
- It acquires the dpkg lock during updates
- This prevents manual package management until it finishes
- On some systems, the scheduler can be overly aggressive

The `systemctl mask` command is the nuclear option—it prevents the service from being started by *anything*,
even if dependencies require it. Use `disable` alone if you want to keep the option to manually start it later.
