# Magisk Overlayfs

- Make system partition (`/system`, `/vendor`, `/product`, `/system_ext`) become read-write. Important: you can only modify content in subdirectories of partitions. It's known that cover entire partitions with overlayfs will cause some problem.
- Use `/data` as upperdir for overlayfs to store modifications. All modifications to overlayfs partition will not be made directly, but will be stored in upperdir, so it is easy to revert.
- Support Magisk version 23.0+ and latest version of KernelSU

## Build

There is two way:
- Fork this repo and run github actions
- Run `bash build.sh` (On Linux/WSL)

## Overlayfs-based Magisk module

- If you want to use overlayfs mount for your module, add these line to the end of `customize.sh`

```bash

OVERLAY_IMAGE_EXTRA=0     # number of kb need to be added to overlay.img
OVERLAY_IMAGE_SHRINK=true # shrink overlay.img or not?

if [ -f "/data/adb/modules/magisk_overlayfs/util_functions.sh" ] && \
    /data/adb/modules/magisk_overlayfs/overlayfs_system --test; then
  ui_print "- Add support for overlayfs"
  . /data/adb/modules/magisk_overlayfs/util_functions.sh
  support_overlayfs
  rm -rf "$MODPATH"/system
fi
```

## Bugreport

- Please include `/cache/overlayfs.log`

## Reset overlayfs

- Remove `/data/adb/overlay` and reinstall module

## Without Magisk

- Possible to test:

```bash
mkdir -p /data/overlayfs
./overlayfs_system /data/overlayfs
```
