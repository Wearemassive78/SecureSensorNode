# Secure Sensor Node

**Project Proposal: Secure Embedded Sensor Node and Microservice Integration**

**Author:** Massimiliano Pupillo  
**Email:** massimiliano.pupillo@studio.unibo.it

## Objective
Design and implement a secure and modular embedded system that collects environmental data via a microcontroller and exposes them through a lightweight microservice.

## Architecture
1. **Embedded Node:**  
   - MCU: STM32 Nucleo board  
   - Sensor: BMP280 (temperature and pressure)  
   - Communication: UART (simulating an insecure channel)  
   - Security: on-device encryption (AES) and hashing (SHA-256)  

2. **Python Microservice:**  
   - Receives encrypted data via PySerial  
   - Decrypts and validates  
   - Exposes a RESTful API using Flask or FastAPI  

## Software Development Lifecycle
- Requirements analysis (functional and non-functional, e.g., security, modularity)  
- System design (embedded + microservice)  
- Implementation (C for firmware; Python for microservice)  
- Testing (unit & integration)  
- DevOps practices: version control (Git), build automation (Makefile, shell scripts), simulated CI/CD  

## Technologies
- **Hardware:** STM32 Nucleo, BMP280 sensor  
- **Firmware:** C, STM32 HAL, embedded cryptography library, UART protocol  
- **Microservice:** Python 3, Flask/FastAPI, PySerial, cryptography/hashlib, logging (JSON)  
- **DevOps & Tools:** Git, Makefile, Bash scripts, virtual environments (venv), Git hooks or GitHub Actions  

GitHub repository (may need cleanup):  
https://github.com/unibo-dtm-se-2223-SecureSensorNode/
