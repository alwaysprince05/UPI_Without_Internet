---
title: UPI Without Internet
emoji: 📡
colorFrom: blue
colorTo: green
sdk: docker
pinned: false
---

# UPI Without Internet (Mesh-Routed Deferred Settlement)

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Java](https://img.shields.io/badge/Java-17%2B-ED8B00?style=flat-square&logo=java&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.3.5-6DB33F?style=flat-square&logo=spring&logoColor=white)

A proof-of-concept Spring Boot application demonstrating how digital payments (similar to UPI) can be securely transmitted in environments without internet connectivity. By leveraging local mesh network concepts (Bluetooth Low Energy, Wi-Fi Direct, Local LAN) and deferred asynchronous settlement, users can queue and relay encrypted transaction payloads that securely sync with the bank when connectivity is restored.

## 🚀 Key Features
* **Mesh Packet Construction:** Compiles transactions into lightweight, signed JSON payloads suitable for BLE/offline transport.
* **Hybrid Cryptography:** Uses RSA (2048-bit) for secure key exchange and AES-256 for fast payload encryption to ensure only the final banking server can decrypt the transaction details.
* **Idempotency & Concurrency:** Handles duplicate mesh packets confidently. Re-delivered offline packets are rejected smoothly, and concurrent settlements are strictly locked to prevent double-spending or race conditions.
* **Interactive Dashboard:** Includes a dynamic web-based dashboard simulating the lifecycle of devices and offline packet relays.

## 🛠 Tech Stack
* **Backend Framework:** Java 17, Spring Boot 3.3.5
* **Database:** H2 (In-Memory) with Spring Data JPA
* **Frontend:** HTML, CSS, JavaScript (Vanilla, Premium Dark UI)
* **Build Tool:** Maven (Wrapper included)

## 📦 Running Locally

You do not need Maven installed on your machine; the project includes the Maven Wrapper.

**Prerequisites:** Java 17+ installed.

1. **Clone the repository:**
   ```bash
   git clone https://github.com/alwaysprince05/UPI_Without_Internet.git
   cd UPI_Without_Internet
   ```

2. **Run the Application (Mac/Linux):**
   ```bash
   chmod +x mvnw
   ./mvnw spring-boot:run
   ```

3. **Run the Application (Windows):**
   ```powershell
   .\mvnw.cmd spring-boot:run
   ```

4. **Access the Dashboard:**
   Open your browser and navigate to `http://localhost:8080`

## 🏗 Architecture Details

1. **Offline Phase:** The sender initiates a transaction. The transaction details are bundled, encrypted via the server's public RSA key (AES key exchange), and signed by the sender.
2. **Mesh Relay Phase:** The payload (`MeshPacket`) is passed between intermediate devices. Devices don't know the contents, they only forward the encrypted blob.
3. **Ingestion & Settlement:** Once a device reaches an internet connection, it pushes the packet to the server's `/api/bridge/ingest` endpoint. The server decrypts it, verifies signatures, checks idempotency, locks the accounts, and settles the funds.

## 👨‍💻 Author
**Prince Maurya (alwaysprince05)**
