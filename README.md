# Grayson Giles (Amnesia)

**Independent Systems Engineer & Architect · Security Researcher · Linux Distribution Maintainer**

**Email: amnesia@lainos.net**

**Co-copyright holder on the official Whonix Bridges wiki (with project founder adrelanos)** · Contributor to **QubesOS** & **Whonix**

**PGP Fingerprint:** 456F 268D 14C9 ECCE 1A77 3558 03E8 F5B6 3BAC 3998

Lead Architect of **lainOS layer 01**, **lainOS layer 02**, the **Protocol 7 System Architecture** · Creator of **VESME-avf**

Independent security researcher and systems architect/engineer specializing in OS-level security, init-system architecture, and privacy-hardened Linux systems. Lead architect of LainOS and Protocol 7, a non-systemd compatibility layer that decouples modern Wayland desktops from systemd-era assumptions, currently in daily production use (stable release 2026.07.29). Diagnosed and repaired critical Snowflake pluggable transport failures in Qubes-Whonix, restoring connectivity for 2,000+ daily users in censored regions; co-author in the official Whonix Bridges wiki. Creator of VESME, an experimental RAM-only secure messaging framework built on Android's Virtualization Framework.

Focused on sovereign computing, deterministic systems, and hardened isolation.

---

## Core Projects

## LainOS Layer 02 ~ OpenRC-Based Operating System

**Role:** Independent Systems & Security Engineer · Lead Architect & Maintainer

**Repository:** [https://forgejo.lain.rocks/lainOS/lainOS-layer-02](https://forgejo.lain.rocks/lainOS/lainOS-layer-02)

LainOS Layer 02 is an OpenRC-based Systemd-free Linux operating system built around the **Protocol 7** compatibility architecture and Arch Linux. It combines a minimal trusted computing base, a modern Wayland desktop, integrated privacy tooling, and distribution-specific administration utilities into a cohesive operating environment designed for daily and specialized use.

### Key Contributions

* Designed and maintain **LainOS Layer 02**, an independent Linux operating system based on Arch Linux and OpenRC.
* Integrated the **Protocol 7** architecture to provide compatibility with modern Linux desktop software without requiring systemd as PID 1.
* Built a complete desktop environment using Sway, greetd, PipeWire, Calamares, and distribution-specific tooling.
* Developed Layer 02 administration utilities for networking, Tor, privacy workflows, sdwdate management, memory hardening, AppArmor, and system configuration.
* Designed documented operational workflows for sensitive sessions, including automated privacy-mode sequencing and verification procedures.
* Integrated optional privacy technologies including Tor, Snowflake, obfs4, sdwdate, WireGuard, hardened_malloc, and secure desktop defaults.
* Maintain Layer 02 as a daily-use operating system, continuously improving usability, installation, documentation, and platform stability.

**Technologies:** Arch Linux · OpenRC · Wayland (Sway) · PipeWire · Tor · WireGuard · Calamares · C · Bash · Apparmor

**Status:** Stable as of 2027.07.30. In daily production use.

## Protocol 7 ~ Init-Agnostic systemd-decoupling Compatibility Layer (LainOS Layer 02)

**Role:** Independent Systems & Security Engineer · Lead Architect & Maintainer

**Repository:** [https://forgejo.lain.rocks/lainOS/Protocol-7](https://forgejo.lain.rocks/lainOS/Protocol-7)

This project was LLM assisted.

Architected and implemented a ~1,900-line C compatibility layer replacing systemd's PID 1, D-Bus session bridge, and notification daemon with OpenRC-based equivalents, enabling a full Wayland desktop stack (Sway, greetd, polkit, Calamares) to operate without systemd while retaining `systemd-libs` for ABI compatibility. Each daemon enforces a drop-privilege-then-seccomp model: root only for initial socket binding, immediate deprivilege to an unprivileged user, syscall allowlisting thereafter.

Security posture is verified, not asserted. Coverage includes `dfuzzer` against the full D-Bus method/property surface, libFuzzer at 2M+ iterations, AddressSanitizer, and Valgrind memcheck, with zero memory-safety findings across all four. Static analysis (`cppcheck`, `semgrep`) resolved the single real finding and correctly ruled out a second as a false positive. A manual, logic-focused review ~ the class of audit fuzzing cannot substitute for ~ identified a missing caller-identity check on a privileged D-Bus method; the gap was diagnosed, fixed, and verified through both allow- and deny-path testing prior to release.

Delivered a Calamares-based graphical installer integrated with a dracut live-boot chain, and a GPG-signed custom package repository (`SigLevel = Required`) for all custom components.

**Technologies:** C, libseccomp, D-Bus, OpenRC, dfuzzer, AddressSanitizer, libFuzzer, Valgrind, Calamares, Wayland (Sway)

**Status:** Stable as of 2027.07.30. In daily production use.

## AppArmor Confinement Suite (lainos-apparmor) ~ Mandatory Access Control for LainOS Layer 02

**Role:** Independent Systems & Security Engineer · Lead Architect & Maintainer

**Repository:** https://forgejo.lain.rocks/lainOS/lainos-apparmor

Designed and shipped a full suite of hardened AppArmor profiles covering the entire LainOS Layer 02 stack ~ networking, DNS, crypto, browsers, media, and Protocol 7's own daemons ~ enforced by default at boot via `enforce.d` symlinks.

**Key Contributions**

* Authored enforced AppArmor profiles for 25+ components, including `dnsmasq`, `unbound`, `dnscrypt-proxy`, `tor`, `stubby`, `iwd`, `dhcpcd`, `chronyd`, `syslog-ng`, `pipewire`, `wireplumber`, `librewolf`, `keepassxc`, `gpg`/`gpg-agent`, `ssh`/`sshd`, `snowflake-pt-client`, and all three Protocol 7 daemons (`lainos-init`, `lainos-dbus-bridge`, `lainos-notifyd`)
* Developed profiles iteratively: observed runtime behavior under normal and edge-case workloads in enforce mode, then audited AppArmor denial logs to trim permissions to the minimum required
* Built a site-override system (`/etc/apparmor.d/local/`) so local policy exceptions never touch shipped profiles
* Designed an emergency-recovery path (`apparmor-emergency-fix`) that drops all Protocol 7 profiles to complain mode for safe boot and diagnosis after a bad enforcement change
* Solved OpenRC-specific edge cases ~ e.g. forcing `syslog-ng` to use explicit `unix-dgram("/dev/log")` to prevent systemd auto-detection(This required creating an OpenRC-compatible syslog-ng fork which skips systemd autodetection.)
* Scoped `gpg`/`gpg-agent` confinement to cover multiple keyring contexts (user, pacman, Tor Browser's isolated GPG homedir) without over-broadening access

**Technologies:** AppArmor, `apparmor_parser`, OpenRC, Bash, Protocol 7

**Status:** Enforced by default in LainOS Layer 02 stable releases.

## DNS Mediation Architecture ~ Stateless, Privacy-Preserving DNS Forwarding

**Role:** Independent Systems & Security Engineer · Architect

**Repository:** [LainOS DNS Mediation Architecture](https://forgejo.lain.rocks/lainOS/lainOS-layer-02/src/branch/main/lainos-dns-mediation-architecture.md)

Designed a layered DNS resolution architecture for LainOS Layer 02 that presents applications with a single stable local resolver (`127.0.0.1`) while abstracting all upstream resolver policy, topology, and transitions inside the operating system.

**Key Contributions**

* Architected a stateless `dnsmasq` forwarding layer (`cache-size=0`, `no-negcache`) that mediates all application DNS traffic without retaining query history
* Designed a three-mode resolution model ~ Plaintext, Encrypted, and Tor-routed Private Mode ~ with deterministic, auditable transitions and zero application-side reconfiguration
* Architected the encrypted-mode chain (`dnsmasq` -> `unbound` -> `dnscrypt-proxy` -> anonymized relay), splitting DNSSEC validation/caching (Unbound) from wire encryption/relay anonymization (dnscrypt-proxy) so that a compromise of one component doesn't expose the other's guarantees
* Built the `lainos-dns` and `private-mode` utilities to orchestrate startup ordering, mode persistence (`/var/lib/lainos/dns-mode`), and safe entry/exit from Tor-routed private mode without DNS leakage
* Ordered the `private-mode` transition sequence (interfaces down -> verify -> save state -> enable Tor/Snowflake/sdwdate -> swap resolver config -> restart -> interfaces up) specifically to prevent leak windows during mode changes
* Brought the full DNS stack under AppArmor confinement (`dnsmasq`, `unbound`, `dnscrypt-proxy`), each restricted to loopback networking and only the filesystem paths required for its role

**Technologies:** dnsmasq, Unbound, dnscrypt-proxy, Tor, AppArmor, Bash, sdwdate

**Status:** Stable, in daily production use as part of LainOS Layer 02.

---

### LainOS ~ Privacy-Hardened Arch Linux (2022 – Present) 

#### 150 GitHub stars(110 stars without the wallpaper repo)

**Role:** Project Lead/Maintainer

**Website:** [https://lainos.net](https://lainos.net)

**Repository:** [https://github.com/The-LainOS-Project](https://github.com/The-LainOS-Project)

LainOS is a rolling-release Linux distribution based on Arch, focused on privacy, user sovereignty, and minimal trusted computing base(layer 02).

**Key Contributions**

* Hardened kernel configuration and MAC randomization by default
* Integrated Calamares installer and configuration with Full disk encryption support
* Maintained long-term rolling release stability
* Led distributed development team and SDLC process

---

### VESME ~ Virtualized Ephemeral Secure Messaging Environment (2025)

**Role:** Architect & Lead Developer

**Repository:** [https://gitlab.com/amnesia1337/vesme-avf](https://gitlab.com/amnesia1337/vesme-avf)

VESME is a secure messaging framework built on Android's Virtualization Framework (AVF) and KVM, providing hardware-backed isolation for ephemeral communications.

**Key Contributions**

* Executed messaging stack inside isolated ephemeral VMs
* Integrated XMPP + OMEMO over Tor
* Implemented GPG-based authentication and secret handling
* Designed traffic obfuscation mechanisms
* Enabled user-initiated secure VM wipe
* Ported framework from aarch64 Debian to x86_64 Arch/lainOS (LESME)

---

### Upstream Contributions

## **QubesOS & Whonix (2024)**

* **Snowflake Pluggable Transport:** Diagnosed, repaired and optimized critical failures in the Snowflake bridge integration within Qubes-Whonix.

* **Quantifiable Impact:** Directly restored and maintained connectivity for **2,000+ daily active users** in heavily censored regions (Russia, Iran, Turkmenistan) who rely on Qubes-Whonix for high-assurance anonymity.

* **Wiki Documentation:** Co-copyright holder (with project founder adrelanos) for the official [Whonix Bridges Wiki](https://www.whonix.org/wiki/Bridges), serving as the primary technical resource for bridge configuration and troubleshooting. **Credited as amnesia at boum dot org**

* **Upstream Integration:** Credited with fixing the "outdated client" bug in `whonix-gateway` templates, ensuring modern Snowflake features (like AMP cache rendezvous) are functional.

**References**

* QubesOS Guide: [https://forum.qubes-os.org/t/quick-start-guide-snowflake-proxy-in-qubes-whonix-tor-control-panel/28889](https://forum.qubes-os.org/t/quick-start-guide-snowflake-proxy-in-qubes-whonix-tor-control-panel/28889)

* Whonix Guide: [https://forums.whonix.org/t/quick-start-guide-fix-snowflake-proxy-in-qubes-whonix-tor-control-panel/20377](https://forums.whonix.org/t/quick-start-guide-fix-snowflake-proxy-in-qubes-whonix-tor-control-panel/20377)

* Whonix Bridges Wiki: [https://www.whonix.org/wiki/Bridges](https://www.whonix.org/wiki/Bridges)

---

## Hardware-Rooted Security Work

* Provisioned Libreboot and Heads via external SPI flashing

* Implemented measured boot with TPM PCR sealing

* Verified firmware integrity using physical GPG tokens

* Neutralized Intel Management Engine during flashing

* Reduced TCB to auditable user-controlled components

---

## Consulting & Applied Security (2017–Present)

Information Security Consultant and Technician providing privacy and security services for individuals and organizations.

**Selected Work**

* Built librebooted Qubes laptops and GrapheneOS devices

* Designed secure network infrastructure and routers (OpenWRT)

* Delivered QubesOS and mobile privacy training

* Implemented endpoint security solutions

* Built secure communication infrastructures

* Replaced invasive IoT systems with privacy-respecting alternatives

---

## Blockchain Infrastructure & Security (2020 ~ 2022)

* Engineered hardened mining clusters with reduced attack surface

* Implemented VPN gateways and encrypted tunnels

* Deployed zero-trust management environment

* Designed cold storage and private key custody

* Conducted electrical and physical safety audits

---

## Technical Skills

**Programming**

* Bash, Python

**Systems & Security**

* OS Architecture and Maintenance

* Security Analysis

* Security and Privacy Engineering

* Virtualization (KVM, Xen, AVF)

* Firmware security (Libreboot, Heads, TPM)

* Tor, PGP, encryption

* Hardened Networking

**Engineering Practices**

* SDLC management

* Manual QA and peer review

* Secure repository management

* Developer key management (GPG)

---

## Community Engagement

* Contributor to open-source security projects

* Co-copyright holder on Whonix Bridges Wiki

* Active participant in Linux and privacy communities

**PGP Fingerprint:** 456F 268D 14C9 ECCE 1A77 3558 03E8 F5B6 3BAC 3998# Grayson Giles (Amnesia)

**Independent Systems Engineer & Architect · Security Researcher · Linux Distribution Maintainer**

**Email: amnesia@lainos.net**

**Co-copyright holder on the official Whonix Bridges wiki (with project founder adrelanos)** · Contributor to **QubesOS** & **Whonix**

**PGP Fingerprint:** 456F 268D 14C9 ECCE 1A77 3558 03E8 F5B6 3BAC 3998

Lead Architect of **lainOS layer 01**, **lainOS layer 02**, the **Protocol 7 System Architecture** · Creator of **VESME-avf**

Independent security researcher and systems architect/engineer specializing in OS-level security, init-system architecture, and privacy-hardened Linux systems. Lead architect of LainOS and Protocol 7, a non-systemd compatibility layer that decouples modern Wayland desktops from systemd-era assumptions, currently in daily production use (stable release 2026.07.29). Diagnosed and repaired critical Snowflake pluggable transport failures in Qubes-Whonix, restoring connectivity for 2,000+ daily users in censored regions; co-author in the official Whonix Bridges wiki. Creator of VESME, an experimental RAM-only secure messaging framework built on Android's Virtualization Framework.

Focused on sovereign computing, deterministic systems, and hardened isolation.

---

## Core Projects

## LainOS Layer 02 ~ OpenRC-Based Operating System

**Role:** Independent Systems & Security Engineer · Lead Architect & Maintainer

**Repository:** [https://forgejo.lain.rocks/lainOS/lainOS-layer-02](https://forgejo.lain.rocks/lainOS/lainOS-layer-02)

LainOS Layer 02 is an OpenRC-based Systemd-free Linux operating system built around the **Protocol 7** compatibility architecture and Arch Linux. It combines a minimal trusted computing base, a modern Wayland desktop, integrated privacy tooling, and distribution-specific administration utilities into a cohesive operating environment designed for daily and specialized use.

### Key Contributions

* Designed and maintain **LainOS Layer 02**, an independent Linux operating system based on Arch Linux and OpenRC.
* Integrated the **Protocol 7** architecture to provide compatibility with modern Linux desktop software without requiring systemd as PID 1.
* Built a complete desktop environment using Sway, greetd, PipeWire, Calamares, and distribution-specific tooling.
* Developed Layer 02 administration utilities for networking, Tor, privacy workflows, sdwdate management, memory hardening, AppArmor, and system configuration.
* Designed documented operational workflows for sensitive sessions, including automated privacy-mode sequencing and verification procedures.
* Integrated optional privacy technologies including Tor, Snowflake, obfs4, sdwdate, WireGuard, hardened_malloc, and secure desktop defaults.
* Maintain Layer 02 as a daily-use operating system, continuously improving usability, installation, documentation, and platform stability.

**Technologies:** Arch Linux · OpenRC · Wayland (Sway) · PipeWire · Tor · WireGuard · Calamares · C · Bash · Apparmor

**Status:** Stable as of 2027.07.30. In daily production use.

## Protocol 7 ~ Init-Agnostic systemd-decoupling Compatibility Layer (LainOS Layer 02)
**Role:** Independent Systems & Security Engineer · Lead Architect & Maintainer
**Repository:** [https://forgejo.lain.rocks/lainOS/Protocol-7](https://forgejo.lain.rocks/lainOS/Protocol-7)

This project was LLM assisted.

Architected and implemented a ~1,900-line C compatibility layer replacing systemd's PID 1, D-Bus session bridge, and notification daemon with OpenRC-based equivalents, enabling a full Wayland desktop stack (Sway, greetd, polkit, Calamares) to operate without systemd while retaining `systemd-libs` for ABI compatibility. Each daemon enforces a drop-privilege-then-seccomp model: root only for initial socket binding, immediate deprivilege to an unprivileged user, syscall allowlisting thereafter.

Security posture is verified, not asserted. Coverage includes `dfuzzer` against the full D-Bus method/property surface, libFuzzer at 2M+ iterations, AddressSanitizer, and Valgrind memcheck, with zero memory-safety findings across all four. Static analysis (`cppcheck`, `semgrep`) resolved the single real finding and correctly ruled out a second as a false positive. A manual, logic-focused review ~ the class of audit fuzzing cannot substitute for ~ identified a missing caller-identity check on a privileged D-Bus method; the gap was diagnosed, fixed, and verified through both allow- and deny-path testing prior to release.

Delivered a Calamares-based graphical installer integrated with a dracut live-boot chain, and a GPG-signed custom package repository (`SigLevel = Required`) for all custom components.

**Technologies:** C, libseccomp, D-Bus, OpenRC, dfuzzer, AddressSanitizer, libFuzzer, Valgrind, Calamares, Wayland (Sway)

**Status:** Stable as of 2027.07.30. In daily production use.

---

### LainOS ~ Privacy-Hardened Arch Linux (2022 – Present) 

#### 150 GitHub stars(110 stars without the wallpaper repo)

**Role:** Project Lead/Maintainer

**Website:** [https://lainos.net](https://lainos.net)

**Repository:** [https://github.com/The-LainOS-Project](https://github.com/The-LainOS-Project)

LainOS is a rolling-release Linux distribution based on Arch, focused on privacy, user sovereignty, and minimal trusted computing base(layer 02).

**Key Contributions**

* Hardened kernel configuration and MAC randomization by default
* 
* Maintained long-term rolling release stability
* Led distributed development team and SDLC process

---

### VESME ~ Virtualized Ephemeral Secure Messaging Environment (2025)

**Role:** Architect & Lead Developer

**Repository:** [https://gitlab.com/amnesia1337/vesme-avf](https://gitlab.com/amnesia1337/vesme-avf)

VESME is a secure messaging framework built on Android’s Virtualization Framework (AVF) and KVM, providing hardware-backed isolation for ephemeral communications.

**Key Contributions**

* Executed messaging stack inside isolated ephemeral VMs
* Integrated XMPP + OMEMO over Tor
* Implemented GPG-based authentication and secret handling
* Designed traffic obfuscation mechanisms
* Enabled user-initiated secure VM wipe
* Ported framework from aarch64 Debian to x86_64 Arch/lainOS (LESME)

---

### Upstream Contributions

## **QubesOS & Whonix (2024)**

* **Snowflake Pluggable Transport:** Diagnosed, repaired and optimized critical failures in the Snowflake bridge integration within Qubes-Whonix.

* **Quantifiable Impact:** Directly restored and maintained connectivity for **2,000+ daily active users** in heavily censored regions (Russia, Iran, Turkmenistan) who rely on Qubes-Whonix for high-assurance anonymity.

* **Wiki Documentation:** Co-copyright holder (with project founder adrelanos) for the official [Whonix Bridges Wiki](https://www.whonix.org/wiki/Bridges), serving as the primary technical resource for bridge configuration and troubleshooting. **Credited as amnesia at boum dot org**

* **Upstream Integration:** Credited with fixing the "outdated client" bug in `whonix-gateway` templates, ensuring modern Snowflake features (like AMP cache rendezvous) are functional.

**References**

* QubesOS Guide: [https://forum.qubes-os.org/t/quick-start-guide-snowflake-proxy-in-qubes-whonix-tor-control-panel/28889](https://forum.qubes-os.org/t/quick-start-guide-snowflake-proxy-in-qubes-whonix-tor-control-panel/28889)

* Whonix Guide: [https://forums.whonix.org/t/quick-start-guide-fix-snowflake-proxy-in-qubes-whonix-tor-control-panel/20377](https://forums.whonix.org/t/quick-start-guide-fix-snowflake-proxy-in-qubes-whonix-tor-control-panel/20377)

* Whonix Bridges Wiki: [https://www.whonix.org/wiki/Bridges](https://www.whonix.org/wiki/Bridges)

---

## Hardware-Rooted Security Work

* Provisioned Libreboot and Heads via external SPI flashing

* Implemented measured boot with TPM PCR sealing

* Verified firmware integrity using physical GPG tokens

* Neutralized Intel Management Engine during flashing

* Reduced TCB to auditable user-controlled components

---

## Consulting & Applied Security (2017–Present)

Information Security Consultant and Technician providing privacy and security services for individuals and organizations.

**Selected Work**

* Built librebooted Qubes laptops and GrapheneOS devices

* Designed secure network infrastructure and routers (OpenWRT)

* Delivered QubesOS and mobile privacy training

* Implemented endpoint security solutions

* Built secure communication infrastructures

* Replaced invasive IoT systems with privacy-respecting alternatives

---

## Blockchain Infrastructure & Security (2020 ~ 2022)

* Engineered hardened mining clusters with reduced attack surface

* Implemented VPN gateways and encrypted tunnels

* Deployed zero-trust management environment

* Designed cold storage and private key custody

* Conducted electrical and physical safety audits

---

## Technical Skills

**Programming**

* Bash, Python

**Systems & Security**

* OS Architecture and Maintenance

* Security Analysis

* Security and Privacy Engineering

* Virtualization (KVM, Xen, AVF)

* Firmware security (Libreboot, Heads, TPM)

* Tor, PGP, encryption

* Hardened Networking

**Engineering Practices**

* SDLC management

* Manual QA and peer review

* Secure repository management

* Developer key management (GPG)

---

## Community Engagement

* Contributor to open-source security projects

* Co-copyright holder on Whonix Bridges Wiki

* Active participant in Linux and privacy communities

**PGP Fingerprint:** 456F 268D 14C9 ECCE 1A77 3558 03E8 F5B6 3BAC 3998
