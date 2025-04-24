# Secure Sensor Node Project Report

## 1. Introduction
The Secure Sensor Node project aims to design and implement a secure, modular embedded system that collects environmental data (temperature and pressure) via an STM32F401RE microcontroller and exposes it through a lightweight Python microservice. The system simulates a scenario in which the communication channel (UART) is insecure, forcing all data protection mechanisms to be implemented at the application layer using AES encryption and SHA-256 hashing.

This project directly addresses key topics from the Software Engineering course, including requirements analysis, architectural design, implementation in C and Python, unit and integration testing, and DevOps practices such as version control (Git), build automation (Makefile and shell scripts), and continuous integration. Additionally, the project lays the groundwork for later exploration of distributed systems concepts—especially socket programming and RESTful API design—making it also relevant to the subsequent Distributed Systems exam.


## 2. Requirements Analysis
- Functional requirements
- Non-functional requirements (security, modularity, scalability)

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
