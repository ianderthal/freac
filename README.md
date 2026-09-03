## Optical drive device path

The USB optical drive's `/dev/srX` path can differ per host (e.g. `/dev/sr1`
on the Proxmox VM, `/dev/sr0` on the mini PC), so it's set via an env var
instead of hardcoded:

```yaml
devices:
  - ${OPTICAL_DRIVE_PATH:-/dev/sr0}:/dev/sr0
```

Set `OPTICAL_DRIVE_PATH` in each Portainer stack's **Environment variables**:

| Host | `OPTICAL_DRIVE_PATH` |
|---|---|
| Homelab | `/dev/sr1` |
| Production | `/dev/sr0` (or omit) |

Verify on the host after deploying:
```bash
docker inspect freac | grep -A 5 Devices
```

Post-deploy setup (manual, not persisted)

fre:ac's settings in `/config` do not survive a fresh deploy — the container's init script resets `freac.xml` to defaults on every startup. Rather than fight this, settings are just set manually after each deploy:

Confirm the USB drive is passed through: Proxmox VM → Hardware tab has a USB Device entry (`13fd:0840`). Add it if missing, then reboot the VM.
In fre:ac's UI, set encoder to FLAC.
Set filename pattern to `<album>/<track(2)> - <title>`.
Confirm output directory is `/output`.

This is only needed after a fresh deploy, not on every container restart.