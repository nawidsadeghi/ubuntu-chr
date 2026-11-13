# Ubuntu VPS to MikroTik CHR Installer

Converts a supported Ubuntu VPS into MikroTik Cloud Hosted Router (CHR) using the official MikroTik RAW image.

## WARNING

This installer performs a destructive disk write.

It permanently erases the current system disk and replaces Ubuntu with CHR.

All files, partitions, operating system data, and user data on the target disk will be destroyed.

Use this script only if you fully understand the impact and have a verified backup.

## Project description

This project provides a fail-safe-first Bash installer for VPS environments that:

- validates Ubuntu version, architecture, and virtualization context
- detects and validates the active system disk
- detects IPv4 and default gateway information
- downloads and verifies the CHR archive before extraction
- requires explicit multi-step user confirmation before any destructive action

The script intentionally aborts when checks are ambiguous.

## Requirements

- Ubuntu Server 20.04 / 22.04 / 24.04 / 26.04 LTS
- x86_64 architecture
- VM environment (KVM / QEMU / Xen / VMware / Hyper-V / compatible cloud VM)
- Legacy BIOS boot mode (not UEFI)
- At least 256 MiB RAM (1024 MiB or more recommended)
- IPv4 default route with detectable interface and gateway
- Root privileges

## Supported CHR version

- MikroTik CHR long-term: **7.23.5**

## Quick install

```bash
curl -fsSL https://raw.githubusercontent.com/nawidsadeghi/ubuntu-chr/refs/heads/main/install-chr.sh \
  -o /tmp/install-chr.sh && \
chmod 700 /tmp/install-chr.sh && \
sudo /tmp/install-chr.sh
```

## What the installer does

1. validates platform and safety preconditions
2. updates Ubuntu packages and installs required utilities
3. resolves and validates the root/system disk
4. detects network data used for post-install reference
5. downloads and verifies the official CHR ZIP archive
6. extracts the `.img` and validates image-vs-disk size
7. performs final confirmation and writes image via `dd`
8. asks for reboot to boot into CHR

## Safety model

- No write is performed before all checks pass.
- Multiple explicit confirmations are required.
- Disk relationship checks ensure the selected target matches the current root disk tree.
- USB-attached removable disks are rejected.
- If any required check fails or is ambiguous, the script aborts.

## Known limitations

- IPv6 is not configured automatically.
- UEFI boot mode is not supported by this workflow.
- The free CHR license has throughput limitations until licensed.

## Repository files

- `install-chr.sh` - main installer script
- `README.md` - usage and operational guidance
- `CHANGELOG.md` - release history
- `SECURITY.md` - security and reporting guidance
- `LICENSE` - licensing terms

## Release/versioning

This project uses a `CHANGELOG.md` with date-stamped releases.

Current version: **1.1.0**.

## Links

- Repository: https://github.com/nawidsadeghi/ubuntu-chr
- Issues: https://github.com/nawidsadeghi/ubuntu-chr/issues
- MikroTik CHR download information: https://mikrotik.com/download/chr
- MikroTik official downloads: https://download.mikrotik.com/routeros/
