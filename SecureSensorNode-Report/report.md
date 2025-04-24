# Secure Sensor Node Project Report

## 1. Introduction
The Secure Sensor Node project aims to design and implement a secure, modular embedded system that collects environmental data (temperature and pressure) via an STM32F401RE microcontroller and exposes it through a lightweight Python microservice. The system simulates a scenario in which the communication channel (UART) is insecure, forcing all data protection mechanisms to be implemented at the application layer using AES encryption and SHA-256 hashing.

This project directly addresses key topics from the Software Engineering course, including requirements analysis, architectural design, implementation in C and Python, unit and integration testing, and DevOps practices such as version control (Git), build automation (Makefile and shell scripts), and continuous integration. Additionally, the project lays the groundwork for later exploration of distributed systems concepts—especially socket programming and RESTful API design—making it also relevant to the subsequent Distributed Systems exam.


## 2. Requirements Analysis

### 2.1 Functional Requirements

- **FR1**: The embedded node shall read temperature and pressure data from the  
  BMP280 sensor at a configurable sampling rate.
- **FR2**: The embedded node shall encrypt and hash the sensor data before  
  transmission.
- **FR3**: The microservice shall receive, decrypt, validate, and store sensor  
  data in a local database.
- **FR4**: The microservice shall expose a RESTful endpoint `/data` returning  
  the latest reading as JSON.
- **FR5**: The microservice shall expose a RESTful endpoint `/history` returning  
  historical data for a specified time range.
- **FR6**: The system shall allow runtime configuration of the sensor’s sampling  
  rate via a dedicated API endpoint.

### 2.2 Non-Functional Requirements

- **NFR1 (Security)**: Ensure confidentiality and integrity using AES-128  
  encryption and SHA-256 hashing on all data.
- **NFR2 (Modularity)**: Maintain a clear separation of concerns between the  
  embedded firmware (`Firmware/`) and the Python microservice (`Microservice/`).
- **NFR3 (Scalability)**: Design the microservice to support horizontal scaling;  
  it must be stateless and compatible with container orchestration.
- **NFR4 (Performance)**: All firmware operations (read–encrypt–transmit) must  
  complete within the configured sampling interval.
- **NFR5 (Maintainability)**: Follow coding standards, include inline comments,  
  and provide API documentation (Swagger/OpenAPI).
- **NFR6 (Reliability)**: The system shall detect and recover from UART  
  communication errors without data loss.


## 3. Development Methodology

### 3.1 Process Model

The project follows an Agile-inspired iterative model, structured in two-week sprints.  
Each sprint includes backlog refinement, sprint planning, implementation, testing,  
and a sprint review. This approach ensures continuous feedback and incremental  
delivery of working software.

### 3.2 Version Control & Branching

We use Git for distributed version control with the following conventions:  
- **Main branch** (`main`): always in a deployable state.  
- **Feature branches**: created per task (`feature/<description>`), merged via pull request  
  after code review.  
- **Commit messages**: formatted as `type(scope): short description`, e.g.,  
  `feat(firmware): add UART encryption module`.

### 3.3 Build Automation

- **Firmware**: a `Makefile` in `Firmware/` defines targets:  
  - `all`: compile the firmware  
  - `clean`: remove build artifacts  
  - `flash`: upload to STM32 via OpenOCD  
- **Microservice**: shell scripts in `Automation/` set up a Python virtual  
  environment (`venv`), install dependencies from `requirements.txt`, and run  
  the service locally.

### 3.4 Continuous Integration

We simulate CI with GitHub Actions (or local Git hooks) defined in  
`.github/workflows/ci.yml`:  
1. **Checkout** code  
2. **Install toolchain** (ARM GCC, dependencies)  
3. **Build firmware** (`make all`)  
4. **Run host-based unit tests** for C code (in `Test/Firmware/`)  
5. **Setup Python venv** and install requirements  
6. **Run Python unit tests** (in `Test/Microservice/`)  
7. **Report** build and test status  

This pipeline enforces early detection of errors and maintains code quality.

## 4. System Design

### 4.1 Architecture Overview

The system consists of two main components: the **Embedded Sensor Node** and the **Python Microservice**.  
The Embedded Sensor Node runs on an STM32F401RE microcontroller, reads data from a BMP280 sensor, encrypts and hashes it, and transmits via UART.  
The Python Microservice receives the data over a serial port, decrypts and validates it, stores it in a database, and exposes it through RESTful APIs.

```mermaid
graph TD
  A[STM32F401RE Node] -->|UART (insecure)| B[Python Microservice]
  B --> C[(Database)]
  B --> D[REST API Clients]

### 4.2 Component Descriptions

Embedded Sensor Node

Hardware: STM32F401RE Nucleo board, BMP280 sensor

Firmware modules:

sensor.c/sensor.h: interfaces with BMP280 via I²C

security.c/security.h: AES-128 encryption & SHA-256 hashing

comm.c/comm.h: UART transmission handler

Python Microservice

Serial Interface: uses PySerial to read encrypted data from UART

Security Module: decrypts with AES-128, verifies integrity with SHA-256

Data Storage: SQLite database for sensor readings

API Layer: FastAPI (or Flask) defining endpoints /data and /history

### 4.3 Communication Channel
We treat UART as an insecure transport: no built-in encryption or checksum.
Therefore all cryptographic protections are implemented at the application layer:

Encryption: AES-128 in CBC mode secures the payload

Integrity: SHA-256 generates a digest appended to the ciphertext


## 5. Implementation
### 5.1 Firmware
- Folder structure (`Firmware/`)
- Toolchain setup
- Key modules (UART, AES/SHA-256)

### 5.2 Microservice
- Folder structure (`Microservice/`)
- API design (endpoints, data formats)
- Security handling (decryption, hashing)

## 6. Testing
- Unit tests (C and Python)
- Integration tests (end-to-end)
- Test automation scripts (`Test/`)

## 7. DevOps & Automation
- Build automation (Makefile, shell scripts)
- Continuous Integration setup (`Automation/`)

## 8. Microservices & REST
- Principles of REST
- API specification (e.g., Swagger/OpenAPI)

## 9. Cloud & Scalability (Optional)
- Deployment considerations
- Scaling strategies

## 10. Maintenance and Future Work
- Potential extensions (sockets, distributed systems)
- Lessons learned

## 11. Conclusion

## 12. Appendices
- Link to GitHub repository
- License text
- Hardware schematics
