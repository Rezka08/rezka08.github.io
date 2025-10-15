---
title: "Software Testing Strategy"
date: 2025-08-25 07:30:00
categories: [Software Testing and Quality Assurance]
tags: [STQA]
image: /assets/images/STQA/strategi-testing.png
---

# Software Testing Strategy

Testing is the process of evaluating software to find defects (bugs) and ensure that the system meets functional and non-functional requirements. Here is a summary of commonly used testing strategies and classifications.

---

### Testing Objectives
- Find and fix defects before release.  
- Verification & validation against requirements.  
- Reduce the risk of failure and increase stakeholder confidence.  
- Ensure security, performance, and user experience.

---

### Software Testing Life Cycle (STLC) — Main Steps
1. **Planning** — create a test plan, identify the environment, estimate time/cost. 
2. **Design** — identify and write test cases, prepare test data.
3. **Execution** — execute test cases (manual/automation), record results.
4. **Reporting & Analysis** — report bugs, severity, and improvement recommendations.
5. **Closure** — evaluate testing coverage and final documentation.

---

### Classification by Abstraction Level
- **Unit Testing**: test the smallest function/method (example: checking the discount calculation function). 
- **Integration Testing**: Integration Testing: test the interaction between modules (e.g., login module ↔ profile).
- **System Testing**: comprehensive system test (functional & non-functional).
- **Acceptance Testing**: test by users/clients for final approval.

---

### Classification by Function
- **Functional Testing** — verify features according to specifications (login, transaction).
- **Non-Functional Testing** — performance, security, reliability, usability.

---

### Classification by Domain
- **Performance Testing** — load/throughput/response time.
- **Security Testing** — check vulnerabilities (SQLi, XSS, etc.).
- **Usability Testing** — ease of use by end-users.

---

### Classification by Structure
- **Black-box Testing** — focus on input/output without looking at the code.
- **White-box Testing** — examine code flow, coverage, and logical paths.

---

### Brief Strategy Examples
- Prioritize test cases for critical features (payment, auth).
- Combine automated tests (unit + integration) for regression.
- Run load tests during the system testing phase before go-live.
- Use a bug priority list (severity/priority) for fixes.

---

### Conclusion
Testing is an essential part of the SDLC to ensure software quality. A combination of good planning, well-written test cases, and structured execution will minimize risks during release.

---

*Brief Reference*: group presentation material (Testing — Group 1).

[View Group 1 Presentation Slides](https://drive.google.com/file/d/1bNFmdW8ePz_z0VM0660SZU4meSBaxc9c/view?usp=sharing)