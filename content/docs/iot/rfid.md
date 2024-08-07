---
title: "RFID"
description: ""
summary: ""
date: 2024-08-07T01:32:31-07:00
lastmod: 2024-08-07T01:32:31-07:00
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## RFID Systems

### Low Frequency (LF)

These systems operate between 30 kHz to 300 kHz, though typically on 125 KHz or 134 kHz. The read speed is slow and very resistant to external interference.

- **Read ranges**: < 10cm
- **Common use cases**: access control and animal control

Related standards:

- ISO 14223 and ISO/IEC 18000-2 - animal control

LF is not considered a standard for global applications as the read power and frequencies vary between countries.

### High Frequency (HF)

These systems operate between 3 and 30 MHz, though typically 12.56 MHz. They are moderately affected by interference.

Common use cases include ticketing, payments, and data transfer.

- **Read ranges**: 10cm to 1m
- **Common use cases**: ticketing, payments, and data transfer

Related standards:

- ISO 15693 - object tracability
- ECMA-340 and ISO/IEC 18092 - NFC
- ISO/IEC 14443 A and ISO/IEC 14443 - smart cards
- JIS C 6319-4 - FeliCa (used in payment systems)

### Ultra High Frequency (UHF)

These systems operate between 300 MHz and 3 GHz. RAIN RFID systems use 860 to 960 MHz. They have very high data transfer speeds and are highly affected by interference. Modern day use of Dipole antennas allow for better reception.

Common use cases include ticketing, payments, and data transfer.

- **Read ranges**: up to 12m
- **Common use cases**: object identification like medications

Related standards:

- ISO 1800-63 - EPC Global Gen2

## RFID Cards

RFID cards store data in binary information. Some cards may have the decimal representation printed on the face of the card. How data is stored on the card and transmitted differs between cards.

The cards can be identified by the antenna using a flash light. High frequency cards have a wider diameter antenna. Low frequency cards have a ring shaped antenna with a thicker width antenna.

Common Types of RFID Cards
T5577 cards - LF 125 khz - asset tagging, animal tracking, asset tracking, designed to be changed and reprogrammed easily
ICCUID cards - HF 13.56 MHz that have a unique identifier - library cards, employee badges
Have MIFARE chip that write the CUID but also the UID. If the target card doesn’t have the chip, you won’t be able to clone the UID.

### T5577

T5577 is LF card on 125 kHz. It's commonly used in "legacy" access control systems like HID Prox. It's also used in animal control and implants.

These cards are designed to be changed and reprogrammed easily. They are still heavily used in access control despite stronger methods existing.

T5577 cards use several types of "air interfaces" that control how data is communicated, e.g., how bits are streamed to the reader, how they are encoded, who talks first (reader vs. card), ect. The T5577 chip can be configured to emulate many types of air interfaces and serial number or ID of each type of card

### ICCUID Cards

CUID is a HF card on 13.56 MHz.

Some cards use a MIFARE chip that stores a UID in addition to a CUID. If the target card doesn't have the chip, the UID cannot be cloned.

## Types of HID cards

### HID Proximity (Prox) Card

Operates on 125 kHz.

Used by majority of access control systems despite iCLASS being considered next gen. Specifically Prox II.

“Px” on the card indicates Prox functionality. Cards can support multiple protocols.

### iCLASS

Higher standard of security than HID Prox. Encrypted with two or three factors, only decodable with a iClass reader (“snoop-proof” credential). Operates on 3.56 MHz or 13.56 MHz

Seos is an optional layer on top of iCLASS that makes cloning the card virtually impossible. It’s possible the organization hasn’t disable HID Prox and cards can be read and emulated that way.

### HID iCLASS Seos 500x

HF card, dual identity.

HID iCLASS Seos + Prox Card 510x: Supports integration of former LF with modern HF with Seos tech

HID 520X iCLASS Seos/iCLASS/Prox

## Cloning a Card

### Search for Card

`lf search`

This command will search for all card types and return the data found.

For a HID card, it will return both the raw data and the formatted data. The raw data can be used for the clone operation.

### Clone Card

`lf em ID_TYPE clone`

This command will clone a card with the provided type.

`lf em 410x clone -id ID_NUMBER`

### Clone Card with HID Prox

Use `lf search` to get the raw value of the HID card.

```bash { title="Search for HID card" }
lf search
```

Write raw data to card:

```bash { title="Write raw data to HID card" }
lf hid clone -r RAW_DATA
```
