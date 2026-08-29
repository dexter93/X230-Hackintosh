<h1 align="center">Hackintosh ThinkPad X230</h1>
<p align="center"> 
    <img src="https://img.shields.io/badge/Status-Working-success?style=flat-square">
    <img src="https://img.shields.io/badge/BIOS-Stock-lightgrey?style=flat-square">
    <img src="https://img.shields.io/badge/CPU-Ivy%20Bridge-informational?style=flat-square">
    <img src="https://img.shields.io/badge/GPU-HD%204000-informational?style=flat-square">
    <img src="https://img.shields.io/badge/Wi--Fi-AR928x-success?style=flat-square">
    <img src="https://img.shields.io/badge/Bluetooth-BCM20702-success?style=flat-square">
</p>
<p align="center">
    <a href="https://www.apple.com/macos/monterey/">
        <img src="https://img.shields.io/badge/Monterey-12.7.6-blue">
    </a>
    <a href="https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_X230.pdf">
        <img src="https://img.shields.io/badge/ThinkPad-X230-blue">
    </a>
    <a href="https://github.com/acidanthera/OpenCorePkg">
        <img src="https://img.shields.io/badge/OpenCore-1.0.7-blue">
    </a>
</p>

<p align="center">
    <a href="">
        <img src="/Pics/Peripherals.png" alt="Lenovo ThinkPad X230 peripherals">
    </a>
    <a href="">
        <img src="/Pics/Monterey.png" alt="Lenovo ThinkPad X230 running macOS Monterey">
    </a>
</p>

Sometimes you just need macOS around to test stuff.

## Personal reminder / toolkit used

This repo is mostly a personal archive, but these are the tools and projects used to build, maintain, and verify the configuration:

* [OpenCore](https://github.com/acidanthera/OpenCorePkg) — bootloader
* [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher) — root patches and legacy hardware support
* [OCAT](https://github.com/ic005k/OCAuxiliaryTools) — OpenCore configuration and kext management
* [Hackintool](https://github.com/benbaker76/Hackintool) — hardware inspection and Hackintosh diagnostics
* [MaciASL](https://github.com/acidanthera/MaciASL) — ACPI/SSDT editing and patching
* [YogaSMC](https://github.com/zhen-zen/YogaSMC) — ThinkPad ACPI/SMC stuff and shortcut handling

YogaSMC works. Some ThinkPad ACPI events aren't mapped though, so a few of the more esoteric buttons don't do anything. The **ThinkVantage button** is one example.

Everything else relevant to this setup is working fine.

This is **pure OpenCore**. There is no old Clover configuration, Clover kext collection, random leftover driver, or decade-old EFI archaeology floating around in here.

Kexts have been brought up to the latest versions used with this configuration and the resulting `config.plist` has been checked and verified with **OCAT**.

In other words: this isn't a 2018 Clover EFI that was dragged through six macOS upgrades and dumped here.

---

## Requirements

* Clean macOS Monterey installation
* OCLP Root Patching

## SIP

SIP is disabled enough for this setup to function.

```text
csr-active-config = AwgAAA==
                   = 0x0803

0x0001 → untrusted kexts
0x0002 → unrestricted filesystem
0x0800 → unauthenticated root
```

That's the lot.

**tl;dr:** `0x0803`, OCLP root patching works, moving on.

## OpenCore Bundle

* **OpenCanopy** — GUI picker
* **OpenRuntime** — Runtime support
* **HfsPlus** — HFS+ filesystem
* **OpenShell** — UEFI shell
* **ResetNvramEntry** — Reset NVRAM
* **ToggleSipEntry** — Toggle SIP

---

## FAQ

### Why Monterey?

**AVX2, mostly. Thanks, Apple.**

The X230 uses a 3rd-gen Ivy Bridge CPU. It supports **AVX**, but **does not support AVX2** — AVX2 was introduced with Intel Haswell.

Could newer macOS versions be made to boot? Probably.

Could you patch your way through increasingly ridiculous compatibility problems? Sure.

**Do I want to? Hell no.**

---

### Hackintosh X230 in 2026? With Monterey?

**Yes. Obviously.**

Is Monterey ancient by 2026 standards? Absolutely. Is the X230 ancient by 2026 standards? Also absolutely. That's kind of the point.

This isn't trying to turn an X230 into a modern Mac. It's a cheap, portable machine that can run macOS well enough to do the occasional thing that requires macOS. Monterey still has a functional Unix userspace, a reasonably modern toolchain, and enough application compatibility to make the machine useful without spending half your life fighting the OS.

The X230 is also unusually well suited to this kind of abuse. The Ivy Bridge platform is well understood, OpenCore does its job, and OCLP takes care of the bits Apple abandoned years ago — most importantly, graphics acceleration and legacy wireless.

**Would I recommend buying an X230 specifically to build a Hackintosh in 2026?**

Probably not.

**Do I already have one sitting around and want macOS on it?**

Absolutely.

Monterey isn't being used because it's new. It's being used because it's the point where the hardware, OS, patches, and general level of bullshit reach an acceptable equilibrium.

**Newer macOS is possible territory.
Monterey is usable territory.**

That's good enough.

---

### Graphics acceleration?

**Yes. OCLP + Ivy Bridge graphics patch.**

The X230's Intel HD Graphics 4000 is obviously not supported by Monterey out of the box. OCLP's Ivy Bridge root patches restore the necessary graphics support and acceleration.

Without acceleration, macOS is basically a slideshow with a Finder attached. With the patches applied, the desktop is properly accelerated and behaves as you'd expect from a usable installation.

---

### Wi-Fi?

**Yes. AR928x with a custom SSDT + OCLP Legacy Wireless patch.**

The stock wireless hardware is based on the Atheros AR928x family. Modern macOS dropped support for this hardware, so the installation uses a custom SSDT alongside OCLP's Legacy Wireless patching to bring it back.

It works.

Which is more than can be said for Apple's support matrix.

---

### Bluetooth?

**Yes. BCM20702. See the included kexts.**

Bluetooth is handled by the BCM20702 USB device and the relevant kexts included in this repository.

---

### YogaSMC?

**Yes. Mostly.**

[YogaSMC](https://github.com/zhen-zen/YogaSMC) handles the ThinkPad-specific ACPI/SMC stuff and works fine with this setup.

There are a few ThinkPad ACPI events that aren't mapped though, so not every shortcut button is going to magically do something just because the hardware has one.

The **ThinkVantage button**, for example, currently does fuck all.

That's an event mapping problem, not YogaSMC falling over. The stuff that's actually mapped works.

If someone eventually maps the missing events, great. If not, there's a perfectly good button on the keyboard that does absolutely nothing. Life goes on.

---

### What doesn't work?

A few things:

* **Mic mute button** — needs to have YogaSMC app running + the fix option enabled in order to work.
* **ThinkVantage button** — YogaSMC doesn't currently have the corresponding ACPI event mapped.
* **ThinkLight** toggle works, but the state is reported back as always Off. Oh well
* **SD card reader** — technically works, but don't trust it.

The SD reader may appear to work, but I wouldn't use it for anything important. Treat it as decorative hardware with occasional aspirations.

There are probably other ThinkPad shortcut/ACPI events hiding in the weeds that aren't mapped either. I haven't bothered chasing every last button because, frankly, the machine works.

---

### Everything else?

Looks and behaves more or less as expected.

The following are working well enough for daily use:

* Sleep
* Display
* Keyboard
* Trackpad
* Audio
* Battery reporting
* USB
* Networking
* The usual ThinkPad bits
* YogaSMC
* Most ThinkPad shortcuts / ACPI events

It's fast, has decent battery life, and is surprisingly usable for hardware that's old enough to have opinions about the economy.

---

### Why do I get no logs on boot?

They're stripped out.

It's stable, and there's no need to dump a wall of OpenCore messages across the screen every time the machine starts.

**Production-level abomination.**

---

### Did you strip the serial numbers and SMBIOS info?

Nope.

Serials, UUIDs, MLB, ROM, model identifiers, the whole SMBIOS identity circus — **it's all still there.**

If you're insane enough to use this EFI anyway, open it in **OCAT**, generate your own SMBIOS/platform information, and replace the existing values.

Don't blindly use mine.

Seriously.

**Generate your own stuff.**

kthxbye.

---

### I need help.

This is my personal repo archive.

Why are you even using this?

---

### But... BUT the other repos for this don't work for me.

You tell me.

---

### WTF is this config?

Something I butchered together.

No warranties. No guarantees. No promises that I understood what I was doing at 2 AM.

---

### Stock BIOS?

**Yes.**

No modified BIOS is required for this configuration.

---

### Any updates planned?

**Hell no.**

This configuration exists because it works. Unless something spectacularly breaks, I'm leaving it alone.

---

## Disclaimer

This is a personal archive of a working ThinkPad X230 Hackintosh configuration.

It is **not** intended to be a polished installation guide, a supported distribution, or a generally recommended way to run macOS.

Hardware configurations vary, macOS versions change, and something that works perfectly on one X230 may decide to explode on another.

If you use this repo, you're on your own.

If it works: great.

If it doesn't:

**welcome to Hackintosh.**
