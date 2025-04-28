# Secure Sensor Node Project Report

## 1. Introduction

The Secure Sensor Node project aims to design and implement a secure, modular embedded system that collects environmental data (temperature and pressure) via an STM32F401RE microcontroller and exposes it through a lightweight Python microservice. The system simulates an insecure UART channel, requiring all cryptographic protections—AES-128 encryption in CBC mode and SHA-256 hashing—to be applied at the application layer.

This project addresses key Software Engineering topics, including requirements analysis, architectural design, implementation in C and Python, unit and integration testing, and DevOps practices (Git, Makefile, CI). It also lays the groundwork for exploring Distributed Systems concepts such as socket programming and RESTful API design.

## 2. Requirements Analysis

### 2.1 Functional Requirements

- **FR1**: The embedded node shall read temperature and pressure data from the BMP280 sensor at a configurable sampling rate.  
- **FR2**: The embedded node shall encrypt (AES-128 CBC) and hash (SHA-256) the sensor data before transmission.  
- **FR3**: The microservice shall receive, decrypt, validate, and store sensor data in an SQLite database.  
- **FR4**: The microservice shall expose a RESTful endpoint `/data` returning the latest reading as JSON.  
- **FR5**: The microservice shall expose a RESTful endpoint `/history` returning historical data for a specified time range.  
- **FR6**: The system shall allow runtime configuration of the sensor sampling rate via an API endpoint.

### 2.2 Non-Functional Requirements

- **NFR1 (Security)**: Ensure confidentiality and integrity with AES-128 encryption and SHA-256 hashing.  
- **NFR2 (Modularity)**: Maintain separation between embedded firmware (`Firmware/`) and Python microservice (`Microservice/`).  
- **NFR3 (Scalability)**: Design microservice to be stateless and support horizontal scaling.  
- **NFR4 (Performance)**: Firmware operations (read → encrypt → transmit) must complete within the sampling interval.  
- **NFR5 (Maintainability)**: Follow coding standards, include inline comments, and provide API documentation (OpenAPI).  
- **NFR6 (Reliability)**: Detect and recover from UART errors without data loss.

## 3. Development Methodology

### 3.1 Process Model

An Agile-inspired iterative model with two-week sprints is adopted. Each sprint includes backlog refinement, planning, implementation, testing, and review, ensuring incremental delivery and continuous feedback.

### 3.2 Version Control & Branching

- **Main branch** (`main`): always deployable.  
- **Feature branches** (`feature/<name>`): per task; merged via pull request after review.  
- **Commit message style**: `type(scope): description`, e.g., `feat(firmware): add UART module`.

### 3.3 Build Automation

- **Firmware**: `Makefile` defines targets: `all`, `clean`, `flash`.  
- **Microservice**: Bash scripts in `Automation/` create a Python `venv`, install dependencies, and run tests.

### 3.4 Continuous Integration

A GitHub Actions pipeline (`.github/workflows/ci.yml`) performs: checkout, toolchain install, firmware build (`make all`), host unit tests, Python environment setup, Python tests, and status reporting.

## 4. System Design

### 4.1 Architecture Overview

The system consists of two main components: the Embedded Sensor Node and the Python Microservice. Data flows from the STM32F401RE (with BMP280) → UART → microservice → SQLite database → API clients.

```mermaid
graph TD
  A[STM32F401RE Node] -->|UART (insecure)| B[Python Microservice]
  B --> C[(SQLite Database)]
  B --> D[REST API Clients]
```
*(Figure 1: High-level system architecture)*

### 4.2 Component Descriptions

**Embedded Sensor Node**  
- **Hardware**: STM32F401RE Nucleo board, BMP280 sensor  
- **Firmware modules**:  
  - `sensor.c`/`sensor.h`: interfaces with BMP280 via I²C  
  - `security.c`/`security.h`: AES-128 encryption & SHA-256 hashing  
  - `comm.c`/`comm.h`: UART transmission handler  
  - `main.c`: initialization and main loop

**Python Microservice**  
- **Modules**:  
  - `serial_comm.py`: reads raw bytes via PySerial  
  - `security.py`: decrypts payload and verifies SHA-256 digest  
  - `database.py`: defines SQLite models and CRUD operations  
  - `api/data.py`: GET `/data` endpoint  
  - `api/history.py`: GET `/history` endpoint  
  - `app.py`: FastAPI setup and router inclusion

### 4.3 Communication Channel

UART is treated as insecure; all encryption and integrity checks occur at the application layer:  
- **Encryption**: AES-128 CBC  
- **Integrity**: SHA-256 digest appended to ciphertext

## 5. Implementation

### 5.1 Firmware

The `Firmware/` directory structure:

```plaintext
Firmware/
├── Makefile            # Build script (all, clean, flash)
├── src/                # Source files
│   └── main.c          # System init and loop
├── inc/                # Header files
│   └── bsp.h           # Board support definitions
└── linker/             # Linker scripts
    └── STM32F401RE.ld  # Flash memory layout
```
**Key files**:  
- **main.c**: initializes clocks, GPIO, and runs main loop  
- **bsp.h/c**: board abstraction layer (LED, UART)  
- **sensor.c/h**: BMP280 driver  
- **security.c/h**: AES & SHA-256 routines  
- **comm.c/h**: UART buffer management

### 5.2 Microservice

The `Microservice/` directory structure:

```plaintext
Microservice/
├── app.py              # FastAPI application entry
├── requirements.txt    # Python dependencies
├── serial_comm.py      # UART interface via PySerial
├── security.py         # Decrypt & hash verification
├── database.py         # SQLite ORM and queries
└── api/                # API endpoint definitions
    ├── data.py         # `/data` endpoint
    └── history.py      # `/history` endpoint
```
**Key files**:  
- **app.py**: bootstraps FastAPI and includes routers  
- **serial_comm.py**: assembles and parses UART messages  
- **security.py**: AES decryption & SHA-256 integrity check  
- **database.py**: stores and retrieves sensor readings  
- **api/data.py** & **history.py**: JSON endpoints

## 6. Testing

### 6.1 Unit Tests

- **Firmware**: host-based tests in `Test/Firmware/` using C mock frameworks  
- **Microservice**: Python tests in `Test/Microservice/` with pytest

### 6.2 Integration Tests

End-to-end testing involves flashing the firmware, running the microservice, and verifying REST API responses against expected data.

## 7. DevOps & Automation

Automation scripts in `Automation/` handle building, flashing, testing, and release packaging. The CI pipeline in `.github/workflows/ci.yml` automates these steps on each push.

## 8. Microservices & REST

This section will cover REST principles, resource modeling, idempotency, and the OpenAPI specification used for documenting the API.

## 9. Cloud & Scalability

(Optional) Discuss containerization with Docker, orchestration with Kubernetes, and horizontal scaling strategies for microservices.

## 10. Maintenance and Future Work

- Socket-based extensions for direct TCP/UDP communication  
- Integration with message queues (e.g., MQTT, RabbitMQ)  
- Blockchain audit trail for tamper-evident logging

## 11. Conclusion

Summarize project outcomes, highlight key learnings, and reflect on challenges and successes.

## 12. Appendices

- **GitHub Repository**: <https://github.com/unibo-dtm-se-2223-SecureSensorNode>  
- **License**: MIT License (see LICENSE file)  
- **Hardware Schematics**: pinout diagrams for STM32F401RE and BMP280 connections
