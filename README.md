# qubes-external-disk
> [!IMPORTANT]
> All usage is at your own risk. The scripts are short, make sure you understand them before you use them.

To prepare this, follow the guide [Qubes OS - Secondary Storage](https://www.qubes-os.org/doc/secondary-storage/), but don't add the device to `crypttab`:

## Improved security by detaching the header
The external disk will already be encrypted, but if you're only going to use
the disk with your Qubes system, you can increase the security by detaching the
header. To do this, create an empty image on `dom0`:
```bash
mkdir ~/.luks_headers
dd if=/dev/zero of=~/.luks_headers/my_external_usb.img bs=16M count=1
```
While following the [Qubes OS - Secondary Storage](https://www.qubes-os.org/doc/secondary-storage/) guide, add `--header ~/.luks_headers/my_external_usb.img` to the arguments to `cryptsetup luksFormat`.

## Usage
- `--description`: Preferrably a unique part of the name of the USB stick, as shown by qvm-usb in dom0.
- `----partition-number`: The partition number to be mounted (e.g. the "1" in "sda1").
- `--luks-name`: The name you chose in `luksFormat`.
- `--logical-volume-name`: The name you chose in `vgcreate`.

To unmount the disk, run `systemctl stop disconnect-qube-usb@DISK_LOGICAL_NAME`.

> [!CAUTION]
> Always make sure that qubes stored on the USB drive are shut down before the disk is unmounted.

> [!WARNING]
> Removing the disk incorrectly will deadlock your system. Note that there is no warning for shutting down `sys-usb` with active disk mounts, so be careful when you update your system that you don't have the disk attached if you reboot `sys-usb`.
