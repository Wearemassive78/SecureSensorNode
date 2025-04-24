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
- Chosen lifecycle model (e.g., Agile, Waterfall)
- DevOps practices and tools overview

## 4. System Design
- High-level architecture diagram
- Component descriptions (Embedded node, Microservice)
- Communication channel (UART insecure)

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
