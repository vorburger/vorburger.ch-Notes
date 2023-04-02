# Linux "tainted" local HW clock

While I was setting up a new laptop, I noticed that `systemd status` printed `Tainted: local-hwclock`, see:

```bash
$ systemctl status
● laptop
    State: running
    Units: 500 loaded (incl. loaded aliases)
     Jobs: 0 queued
   Failed: 0 units
    Since: Sat 2023-04-01 20:31:56 CEST; 1h 36min left
  systemd: 253.2-1.fc38
  Tainted: local-hwclock
   CGroup: /
...
```

The reason for that is further explained by this:

```bash
$ timedatectl
               Local time: sam 2023-04-01 19:02:13 CEST
           Universal time: sam 2023-04-01 17:02:13 UTC
                 RTC time: sam 2023-04-01 19:02:13
                Time zone: Europe/Zurich (CEST, +0200)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: yes

Warning: The system is configured to read the RTC time in the local time zone.
         This mode cannot be fully supported. It will create various problems
         with time zone changes and daylight saving time adjustments. The RTC
         time is never updated, it relies on external facilities to maintain it.
         If at all possible, use RTC in UTC by calling
         'timedatectl set-local-rtc 0'.
```

As it says, this is fixed by doing:

```bash
$ timedatectl set-local-rtc 0

$ timedatectl
               Local time: sam 2023-04-01 19:07:57 CEST
           Universal time: sam 2023-04-01 17:07:57 UTC
                 RTC time: sam 2023-04-01 17:07:57
                Time zone: Europe/Zurich (CEST, +0200)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no

$ systemctl
# Does not complain about "Tainted:" anymore now.
```
