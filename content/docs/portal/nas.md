---
title: "NAS"
description: ""
summary: ""
date: 2024-08-05T20:48:27-07:00
lastmod: 2024-08-05T20:48:27-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## SATA Connections

SATA cables come in three varities: data only, power only, and data plus power. They are available with male and female connectors.

### SATA Data Connectors

SATA data connections use seven pins - four pins in two pairs for data plus three ground pins. Standard SATA connections are L-shaped and eSATA uses a straight port.

![sata connections](images/sataPorts.png)

### SATA Power Connectors

SATA power connectors uses 15 pins, which specific to the SATA standard. They are often accompanied by a 4-pin Molex LP4 connector that connects to a compuwer power supply.

<!-- ![sata connections](images/sataPower.png) -->

{{< picture src="images/sataPower.png" alt="A bird flying through a field of tall grass" >}}

The 15-pin connector supplies 3.3V in addition to the traditional 5 V and 12 V, though it's rarely used in practice.

SATA cables have a maximum length of 1 meter (3.3 ft.).

### Power Disable Feature

SATA 3.3 and newer provides for a power disable feature (PWDIS). In older versions of the SATA spec, pins 1-3 carried the 3.3 V rail to the drive. In SATA 3.3, pin 3 (P3) of the power connector is now power independent and sends a power disable feature. This allows the computer to restart the drive.

The downside is a PSU with older SATA connectors supplying 3.3V on P3, will continuously send a high state signal on P3, meaning the drive will never start. It is stuck in a hard reset condition and the drive won't spin up.

![sata connections](images/sataPowerPinout.png)
