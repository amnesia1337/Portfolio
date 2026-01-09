# Grayson Giles (Amnesia) – Portfolio

**Hardened Systems Architect & Security Researcher**

Co-copyright holder on the official **Whonix Bridges wiki** (shared with founder adrelanos) · Lead Architect of **LainOS** (modular, privacy-hardened Arch derivative with Protocol 7 init decoupling) · Creator of **VESME** (experimental mobile messaging via Android Virtualization Framework) 

Engineering systems that treat security as a fundamental property of the architecture, not an added feature. By leveraging hardware-backed virtualization, firmware hardening, and modular OS design, Grayson Giles creates environments where hardened isolation and integrity-verified security are the default state.

---

## **Architectural Framework: Full-Stack System Hardening** 🏗️

### **Layer 0: Platform Security(Hardware Root of Trust & Firmware Integrity)**

*Focuses on the elimination of "Black Box" components and the enforcement of a verified boot path.*

* **Heads Implementation & Measured Boot:** Deployment of **Heads** as a specialized bootloader residing in the SPI flash. Utilizing **TPM (Trusted Platform Module)** and GPG-based signing to engineer a measured boot process that detects unauthorized firmware or kernel modifications via physical security tokens.
* **Libreboot Implementation:** Reclamation of hardware sovereignty through the replacement of proprietary, closed-source firmware with **Libreboot**, ensuring the lowest level of the system is fully auditable.
* **Intel ME Neutralization:** Utilization of blob-free firmware to physically and logically neutralize the Intel Management Engine.


### **Layer 1: Virtualization & Domain Isolation**

*Ensures that system components remain isolated through hardware-backed memory protection and hypervisor boundaries.*

* **VESME (Virtualized Ephemeral Secure Messaging Environment):** Architectural design of a communication framework leveraging the **Android Virtualization Framework (AVF)** and **KVM**. Messaging stacks are executed in isolated, ephemeral VMs to provide hardware-backed memory protection.
* **QubesOS & Whonix Integration:** Implementation of the **Xen Hypervisor** to enforce strict security boundaries between untrusted networking domains and secure vault environments.
* **Upstream Contributions:** Diagnosis and optimization of the **Snowflake** pluggable transport for Qubes/Whonix, directly improving the anti-censorship and anonymity capabilities for the global user base.
* **LESME (LainOS Ephemeral Secure Messaging Environment):** Successful **cross-architecture migration** of the VESME properties from `aarch64` to `x86_64`, integrating virtualized ephemeral messaging into the LainOS core.

### **Layer 2: Operating System Design & Logic (LainOS)**

*Focuses on the construction of a modular, hardened OS environment decoupled from monolithic upstream constraints.*

* **Protocol 7 (Logic Interception):** Engineering of a custom repository priority stack for Layer 02 of the LainOS ecosystem. Utilizes `libalpm` versioning math to establish a logical "ceiling," ensuring the modular core remains update-resistant and stable against upstream reverts.
* **Modular Infrastructure:** Development of **LainOS**, a rolling-release environment built on Arch Linux that prioritizes user privacy through standardized MAC randomization and hardened kernel configurations as fundamental system properties.

---

## Key Projects 🛠️

### **Protocol 7: Modular Systems Architecture (LainOS Layer 02)** 🏗️ (2026)

**Role:** Systems Architect

**Description:** The structural foundation for **Layer 02** of the LainOS ecosystem. It utilizes a custom architectural blueprint to enable a pure, rolling-release Arch Linux environment that operates independently of default init-system monoliths.

* **Hierarchical Logic Interception:** Engineered a repository priority stack (`[protocol7]`) that allows for surgical package overrides while maintaining full binary compatibility with upstream mirrors.
* **Version Precedence (Epoch Logic):** Utilized `libalpm` versioning math to establish a logical "ceiling" (Epoch: 1), ensuring the system's modular core remains update-resistant and stable against upstream reverts.
* **ABI-Compatible Simulation:** Implemented the "Signal Forge"—a series of behavioral shims and Unix domain socket mocking that simulates expected system handshakes, ensuring application stability in a decoupled environment.

**Repository**: [Protocol 7 on GitLab](https://gitlab.com/lainos/protocol-7)

### VESME (Virtualized Ephemeral Secure Messaging Environment) 💬 (2025)

* **Description**: A communication framework designed for secure messaging within the Linux Terminal Application of Android, which runs within the Android Virtualization Framework. VESME aims to provide a pioneering foundational blueprint for hardened mobile messaging on mobile devices using virtualization, encryption, and ephemeral computing, designed for those facing heavy surveillance or censorship.

* **Key Features**:
    * Designed to look inconspicuous(like taking notes etc...) and 'hide' within the terminal application.
    * Multi-layered security architecture that aims to align with ISO and NIST standards.
    * Utilizes PGP for XMPP account authentication and OMEMO encrypted XMPP over Tor for secure messaging.
    * Ensures no residual data remains after use through user initiated manual VM wipe. Toggle terminal off in developer options for instant data wipe, reinstall terminal for thorough overwrite(10-15 min).
    * Integrates traffic obfuscation techniques to enhance privacy.
    * A pioneering example of ephemeral computing utilizing virtualization in mobile security.
    * Future plans include a CLI fork of Ricochet Refresh for integration into VESME, to offer a serverless choice.

* **Cross-Architecture Port and LESME Integration (Achievement Highlight)** 🚀 (2025)
    * **Architecture Migration:** Successfully led the complex migration of the VESME framework and its security properties from its original **aarch64 Debian 12 VM** environment to the **Arch Linux/LainOS x86\_64** platform.
    * **System Integration:** This port enabled the creation of the **LainOS Ephemeral Secure Messaging Environment (LESME)**, which deeply integrates VESME's secure tools into the LainOS base.
    * **Demonstrated Expertise:** Required overcoming significant library, kernel, and virtualization compatibility challenges to maintain system stability identical functionality across disparate Linux distributions.

* **Repository**: [VESME on GitLab](https://gitlab.com/amnesia1337/vesme-avf)

### LainOS 🖥️ (2022 - Present)

As the lead developer of **LainOS**, a Linux distribution inspired by the anime "Serial Experiments Lain," Grayson has played a pivotal role in shaping a secure and privacy-focused operating system with cyberpunk and anime aesthetics aimed at developers, tinkerers, and hackers. Key contributions include:

* **Secure Architecture**: Developed a system that prioritizes user privacy and integrates various security features such as MAC randomization by default, making it a reliable choice for users concerned about their digital footprint.
* **Community-Driven**: Collaborated with a global community of developers to emphasize user security and privacy, fostering an inclusive environment for innovation.
* **Technical Specifications**: Based on **Arch Linux**, featuring lightweight window managers like **Hyprland** and **Openbox**.
* **Tools and Utilities**: Includes essential tools for network analysis (Wireshark, Nmap), exploitation (Metasploit), and privacy (Tor, I2P, Keepass, Kleopatra, Kloak).
* **Website**: [LainOS Official Site](https://lainos.dev) | **Repository**: [LainOS on GitHub](https://github.com/The-LainOS-Project)
* Distrowatch link: https://distrowatch.com/lainos

### Contributions to QubesOS and Whonix 🔒 (2024)

* **Co-copyright holder** on the official Whonix Bridges wiki page (copyright shared with project founder adrelanos/ENCRYPTED SUPPORT LLC), for upstream optimizations to Snowflake Tor pluggable transport in **Whonix/Qubes**.

* **QubesOS**: Diagnosed, repaired, and optimized the broken Snowflake Tor pluggable transport feature, while significantly improving its reliability, thus improving the anti-censorship and user anonymity capabilities of QubesOS.
    * **Guide**: [Quick Start Guide for Snowflake Proxy in Qubes/Whonix](https://forum.qubes-os.org/t/quick-start-guide-snowflake-proxy-in-qubes-whonix-tor-control-panel/28889)
* **Whonix**: Diagnosed, repaired, and optimized the broken Snowflake Tor pluggable transport feature in Whonix Tor control panel, thus improving the anti-censorship and user anonymity capabilities of Whonix, featured and credited on the Whonix Bridges Wiki page.
    * **Guide**: [Quick Start Guide for Fixing Snowflake Proxy in Qubes/Whonix](https://forums.whonix.org/t/quick-start-guide-fix-snowflake-proxy-in-qubes-whonix-tor-control-panel/20377)
    * **Whonix Wiki Contribution**: Featured as a contributor on the [Whonix Bridges Wiki Page](https://www.whonix.org/wiki/Bridges), credited as "Amnesia <`amnesia at boum dot org`>"

---

## Additional Experience and Contributions

#### Information Security Consultant and Technician (2017 - Present)

In this role, a range of services is provided that enhance privacy and security for individuals and organizations. Key services include:

* **QubesOS and Mobile Device Privacy Training**: Conduct training sessions focused on QubesOS and GrapheneOS, educating users on security features and best practices.
* **Secure Device Development and Sales**: Build and sell librebooted Qubes laptops, GrapheneOS mobile devices, and OpenWRT/DD-WRT routers, ensuring clients have access to secure hardware.
* **Secure Network Infrastructure**: Design and implement secure networks tailored to client needs, teaching the importance of endpoint security.
* **Endpoint Security Solutions**: Provide customized endpoint security solutions to protect client systems from vulnerabilities and attacks.
* **Open Source Home Security Solutions**: Develop and implement open-source home security solutions, including camera systems that prioritize user privacy.
* **Communication Infrastructure**: Build secure and private communication infrastructures for clients, ensuring confidentiality and protection from unauthorized access.
* **IOT Eradication**: Helping people understand which proprietary devices are violating their privacy in their homes, and replacing them with alternatives that don't.

---

## Technical Skills 🧠

* **Programming Languages**: **Bash** and **Python** (used to develop custom tools and infrastructure for the **VESME** and **LainOS** projects).
* **Security-Hardened BASH Engineering (LESME Script)** 🔐:
    * Developed the complex **LESME setup script** to securely provision the LainOS messaging environment, demonstrating a commitment to secure development practices.
    * Features **secure passphrase handling** by utilizing GPG's `--passphrase-fd` with file descriptors, preventing sensitive data from being stored in shell history or visible in process lists.
    * Integrates the `pass` utility to manage and decrypt XMPP passwords using the generated GPG key, ensuring a robust chain of trust.
    * Automates critical security steps, including **Tor/obfs4 bridge configuration** and **OpenNTPD** setup for reliable time synchronization.
* **Security Protocols**: Knowledgeable in Tor, PGP, various encryption techniques, and best Network Security practices, ensuring robust security measures in projects; see LainOS, QubesOS, and Whonix contributions listed above along with the VESME project.
* **Operating Systems**: Extensive experience in developing and securing Linux-based operating systems, contributing to their reliability and performance; see QubesOS and Whonix contributions listed above.
* **Rolling Release Linux Development and Maintenance**: See the LainOS project listed above.
* **Secure Hardware and Software Implementation**: See information security consultant work listed above.
* **Research and Analysis**: Strong analytical skills for identifying vulnerabilities and developing innovative security solutions, such as the VESME project listed above.

### **SDLC & Engineering Leadership** ⚙️

* **SDLC Management & Oversight:** Developed and enforced a structured **Software Development Life Cycle (SDLC)** for the **LainOS** distribution, ensuring all development phases (planning, design, implementation, and quality assurance) were tracked and completed before release.
* **Manual Quality Assurance (QA):** Directed the testing and development lifecycle through **manual integration** and **rigorous peer code review** before merging. This process ensured system stability, security, and adherence to design specifications.
* **Team and Process Leadership:** Led the LainOS development team, establishing core development processes including standardized **communication methods**, secure **repository management**, and **developer encryption key management** (using PGP/GPG) to maintain the integrity of the open-source project.

---

## **Blockchain Infrastructure & Security** 🔒⛏️

**(2020 – 2022)**

**Hardened Operations & Threat Mitigation:** Defended high-value computational assets against remote exploitation and network-level interception. Orchestrated a Zero-Trust management environment (SSH-key-only, non-standard port tunneling) to ensure zero unauthorized access events over a 24-month period.

* **Secure Rig Architecture:** Engineered high-performance mining clusters optimized for thermal stability, power efficiency, and **attack surface reduction**.
* **Network Privacy:** Implemented hardened VPN gateways and encrypted tunnels to obfuscate mining traffic and prevent ISP-level metadata leakage.
* **Hardened Operations:** Deployed integrated firewalls and SSH-key-only management to secure hardware against remote exploitation.
* **Asset Custody:** Designed **Cold Storage** solutions and private key management protocols (GPG/Air-gapped) to ensure long-term financial integrity.
* **Risk Mitigation:** Conducted safety audits covering electrical load management and physical hardware security to prevent catastrophic failure.

---

## Community Engagement 🤝

* Actively contributes to open-source projects and engages with users to gather feedback, demonstrating a commitment to continuous improvement and collaboration within the information security and Linux communities.
* Grayson Giles aka amnesia1337 PGP fingerprint: 2B53ECEF5A47ACF19A080E46B2E5012D409A7AFB on keyserver [https://keys.openpgp.org](https://keys.openpgp.org)


