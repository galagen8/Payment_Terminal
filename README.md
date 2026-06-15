# Payment Donation Terminal

A hardware and software solution designed as an automated donation terminal for a church. The system accepts cash (banknotes) and triggers electrical decorative candles to light up for a specific duration based on the donation amount.

![Finished Donation Terminal in Installation](название_твоего_главного_фото.jpg) <img width="3468" height="4624" alt="20251226_145702" src="https://github.com/user-attachments/assets/0ebac5e1-40f9-47f1-90ed-64596b04e598" />



This repository contains the complete design files, including MCU firmware, PCB layouts, and 3D mechanical models.

---

## Tech Stack & Tools

* MCU & Firmware: STM32G030K8T6 (Production), NUCLEO-F446RE (Prototyping) using STM32CubeHAL
* ECAD: Altium Designer
* MCAD: SolidWorks
* Peripherals: ICT NK77 Bill Acceptor

---

## Features

* Banknote Validation: Integrated with an ICT NK77 Bill Acceptor using a reliable pulse-based protocol.
* Cost-Optimized Hardware: Built on a budget-friendly and reliable STM32G0 MCU, utilizing STM32CubeHAL for seamless firmware portability from the initial STM32F4 prototype.
* Hardware-Driven Power Gating: Candles are hardwired and controlled via low-side NPN transistor switches, eliminating wireless reliability issues and fast battery discharge common with infrared (NEC-protocol) remotes.
* Scalable Output: The custom PCB features parallel sockets for each channel, supporting up to 6 candles in total (powered by a dedicated stable 5V PSU).

### PCB Design & Assembly

*Реальная собранная плата:*
![Custom PCB Assembly](1765641905588.jpg)

*Топология / 3D-визуализация в Altium Designer:*
![Altium Designer PCB Layout](название_файла_альтиум.jpg)

* Secure Enclosure: Features a custom-designed sheet metal chassis with an integrated metallic safe deposit box and physical lock. The terminal utilizes a multi-rail power system to supply the necessary 12V for the bill acceptor and a separate regulated rail for the logic control board.

### Enclosure Design & System Sub-Assembly

*3D-модель корпуса в SolidWorks:*
![SolidWorks Enclosure Design](название_файла_солид.jpg)

*Внутренний монтаж системы и блоки питания:*
![System Sub-Assembly and Power Management](20251219_131434.jpg)

---

## How It Works & Algorithm

### 1. Power-On Self-Test (POST) / Diagnostics
Upon boot, the system runs a quick diagnostic routine: all candles light up sequentially for 1 second each to verify wiring, transistor switching, and LED functionality before entering normal operation mode.

### 2. Pulse Interfacing
The ICT NK77 Bill Acceptor is configured via DIP switches to use the pulse protocol, transmitting 1 pulse per 10 THB (e.g., a 100 THB banknote generates 10 pulses). The STM32 MCU captures and processes these incoming pulses.

### 3. Candle Control Logic
The lighting sequence dynamically scales with the donation amount, managed by a rolling timer:
* 1st Level Donation: The 1st candle lights up for 10 minutes.
* 2nd Level Donation: If another bill is inserted while the 1st candle is active, the 2nd candle turns on, and the 10-minute countdown resets for both.
* 3rd Level Donation: Subsequent insertions activate the 3rd candle and renew the master 10-minute timer.

---

## Repository Structure

* /Firmware — STM32CubeIDE project and source code (HAL-based).
* /Altium — Schematic and PCB layout files (manufactured via JLCPCB, hand-assembled).
* /SolidWorks — 3D models and manufacturing drawings for the sheet metal enclosure and safe box.
