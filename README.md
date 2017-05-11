# perc-status

This is a simple script that takes the information about a Dell
PowerEdge RAID Controller (PERC) from Dell OpenManage Server
Administrator and presents that information is a more succinct way
than `omreport` does.  Its output is strongly influenced by ZFS's
`zpool status` command.

## Usage

You need to have OpenManage Server Administrator installed.

Simply run the `perc-status` script.  If `omreport` isn't in your
path, pass its location to the script with the `-r` command line
parameter.

You can limit the program's output to a single controller with the
`-c` command line parameter.

The `-b` command line parameter will show the bus protocol (SAS, SATA,
IDE, etc.) for each physical disk.  Similarly, the `-m` parameter will
show each disk's model number.

## Examples

A few examples of the script's output:

```
PERC H310 Mini (0)  Ready  Ok
  Virtual Disk 0 (0) at /dev/sda
    RAID-1    Ready   Ok  931.00 GiB
      0:1:0   Online  Ok  931.00 GiB
      0:1:1   Online  Ok  931.00 GiB
```

```
PERC 6/E Adapter (1)  Degraded  Non-Critical
  data (0) at /dev/sdb
    RAID-60      Ready   Ok    9.09 TiB
      Span 0
        0:0:0    Online  Ok  931.00 GiB
        0:0:1    Online  Ok  931.00 GiB
        0:0:2    Online  Ok  931.00 GiB
        0:0:3    Online  Ok  931.00 GiB
        0:0:4    Online  Ok  931.00 GiB
        0:0:5    Online  Ok  931.00 GiB
        0:0:6    Online  Ok  931.00 GiB
      Span 1
        0:0:7    Online  Ok  931.00 GiB
        0:0:8    Online  Ok  931.00 GiB
        0:0:9    Online  Ok  931.00 GiB
        0:0:10   Online  Ok  931.00 GiB
        0:0:11   Online  Ok  931.00 GiB
        0:0:12   Online  Ok  931.00 GiB
        0:0:13   Online  Ok  931.00 GiB
    Spares
      0:0:14     Ready   Ok  931.00 GiB
```

```
PERC 6/E Adapter (1)  Ready  Ok
  Virtual Disk 0 (0) at /dev/sdb
    RAID-5    Degraded    Non-Critical    8.18 TiB
      1:0:0   Online      Ok            698.12 GiB
      1:0:1   Online      Ok            698.12 GiB
      1:0:2   Online      Ok            698.12 GiB
      1:0:3   Online      Ok            698.12 GiB
      1:0:4   Online      Ok            698.12 GiB
      1:0:5   Online      Ok            698.12 GiB
      1:0:6   Online      Ok            698.12 GiB
      1:0:7   Online      Ok            698.12 GiB
      1:0:8   Online      Ok            698.12 GiB
      1:0:9   Online      Ok            698.12 GiB
      1:0:10  Online      Ok            698.12 GiB
      1:0:11  Online      Ok            698.12 GiB
      1:0:13  Rebuilding  Ok            698.12 GiB  20%
  Virtual Disk 1 (1) at /dev/sdc
    RAID-5    Ready       Ok              8.18 TiB
      0:0:0   Online      Ok            698.12 GiB
      0:0:1   Online      Ok            698.12 GiB
      0:0:2   Online      Ok            698.12 GiB
      0:0:3   Online      Ok            698.12 GiB
      0:0:4   Online      Ok            698.12 GiB
      0:0:5   Online      Ok            698.12 GiB
      0:0:6   Online      Ok            698.12 GiB
      0:0:7   Online      Ok            698.12 GiB
      0:0:8   Online      Ok            698.12 GiB
      0:0:9   Online      Ok            698.12 GiB
      0:0:10  Online      Ok            698.12 GiB
      0:0:11  Online      Ok            698.12 GiB
      0:0:12  Online      Ok            698.12 GiB
    Spares
      0:0:13  Ready       Ok            698.12 GiB
  Global Spares
    0:0:14    Ready       Ok            698.12 GiB
  Unused
    1:0:12    Clear       Ok            698.12 GiB  18%
    1:0:14    Ready       Ok            698.12 GiB
```

## Bugs

This script relies on the undocumented XML output from `omreport`.
Meanings of various things have been reverse engineered and are likely
to be incomplete.

In particular, device states and statuses are represented as plain
integers in the XML.  The script contains mappings for all of the
values known to the author, but there are many values still
unaccounted-for.

If you run the script and it shows a value of "Unknown" with a number
next to it, please email phil_g@pobox.com with the output of the
script and the output of `omreport` for the device in question.  (If
in doubt about the specific device, simply capture the entire output
of `omreport storage controller controller=_x_`, where "_x_" is the
number of the controller to which the device is attached.)
