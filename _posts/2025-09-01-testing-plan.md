---
title: "Testing Plan"
date: 2025-09-01 07:30:00
categories: [Software Testing and Quality Assurance]
tags: [STQA]
image: /assets/images/STQA/testing-plan.png
---

# Testing Plan

This article is a structured, practical Test Plan template and explanation that you can reuse or adapt for any software project. It summarizes the essential sections of a test plan and explains what to include in each section so that testing activities are consistent, measurable, and traceable. The structure follows the common guidance of IEEE-829-style test planning.

<p align="center">
  <img src="/assets/images/STQA/IEE.png" alt="Strategi Testing" style="max-width:50%;height:auto;">
</p>

## 1. Plan Identifier
A unique identifier or version number for this test plan. Use it to track revisions and to distinguish this plan from other projects or subsequent versions. Include version history and authors. 

## 2. References
List documents, standards, and sources that the test plan depends on (requirements spec, design docs, classification standards, IEEE test documentation templates, etc.). References ensure consistency with project artifacts. 

## 3. Introduction / Purpose
A short executive summary describing the purpose, objectives, and scope of the testing effort. Explain why the plan exists and what the testing aims to validate. This section sets stakeholder expectations. 

## 4. Test Items (Scope)
Specify the software components, modules, features, or artifacts that will be tested. Be concrete: list modules, pages, APIs, or integrations that are in-scope. This helps avoid ambiguity during execution. 

## 5. Features to be Tested
Describe, from an end-user perspective, which features will be verified and the general acceptance criteria for those features. Prioritize features by risk and business impact. 

## 6. Features Not to Be Tested
List excluded features and the reasons for exclusion (out of scope, stable/legacy, deferred to a later release). Explicit exclusions reduce misunderstandings about test coverage. 

## 7. Approach / Testing Strategy
Explain the overall strategy:
- Types of testing to be performed (unit, integration, system, acceptance).  
- Test techniques to use (black-box, white-box, gray-box).  
- Manual vs automated testing approach, tools, and test data strategy.  
- Pass/fail criteria for items and overall testing.  
Design the approach so it answers: *what we test, how we test, why we test it this way.* 

## 8. Pass / Fail Criteria
Define measurable pass/fail rules for test items and for the whole testing phase (for example: all critical flows pass; no blocking defects; ≥ 95% test-case pass rate). Clear criteria avoid subjective go/no-go decisions. 

## 9. Suspension & Resumption Criteria
State conditions that will suspend testing (e.g., app crashes, data corruption, critical defects) and the requirements that must be met to resume (defect fixes verified, environment restored). Documenting these prevents wasted effort during catastrophic failures. 

## 10. Test Deliverables
List artifacts that the testing team will produce and hand over: test cases, test execution results, bug reports with reproduction steps and screenshots, test summary report, and final approval sign-offs.

## 11. Remaining Tasks
Detail remaining work items at the time of status reporting (test cases to write, retests pending, environment setup tasks). This helps track progress to completion.

## 12. Environmental Needs
Specify the hardware, OS, browser versions, test data, credentials, network configuration, and any external systems that must be available for valid testing runs. Include exact versions to make tests reproducible.

## 13. Responsibilities
Assign roles and responsibilities (Test Manager, Testers, Developers, Release Manager). Define who writes test cases, who executes them, who triages defects, and the escalation path.

## 14. Staffing and Training Needs
List required skills and proposed training (e.g., test tool onboarding, domain knowledge). Identify backup testers to reduce single-person risk.

## 15. Schedule
Provide a timeline for test planning, test case design, execution cycles, retesting, and final sign-off. Include milestones and dependencies on development builds.

## 16. Risks, Contingencies, and Mitigations
Identify likely risks to testing (missing builds, ambiguous requirements, environment instability) and proposed mitigations (backup environments, extra test resources, clarification sessions).

## 17. Glossary & Abbreviations
Define terms used in the plan so all stakeholders share a common understanding (e.g., “Test Plan Identifier”, “Test Items”, “Pass/Fail Criteria”).

## 18. Approvals
Record who must approve the plan and capture names, roles, signatures, and dates for traceability.

---

### How to use this template
You can also access and duplicate the editable test plan template in Google Docs [here](https://docs.google.com/document/d/1F1TUX5BkviRbw8auI2Xq2K2wyGTS-Ag14DRymiELmzc/edit?usp=sharing).

[View Group 3 Presentation Slides](https://drive.google.com/file/d/1Pjx6n08veg7o6P-4LjazQCGOx29xH-YS/view?usp=sharing)

---

## **BMI Calculator — Test Plan (Cross-Platform)**

This test plan targets the web-based BMI Calculator. It defines scope, risks, test items, approach, pass/fail rules, deliverables, and the execution plan for cross-browser verification on Windows and macOS. The plan is based on the project test plan document and refines it into executable QA work. 

### Test Items
- Input validation for weight (kg) and height (cm).  
- BMI calculation engine using formula: `BMI = weight(kg) / (height(m))^2`.  
- Health classification logic (Underweight, Normal, Overweight, Obese).  
- UI components and error messaging.  
- Reset/clear input functionality.  
- Output formatting and readability. 

### Supported Platforms & Browsers
- Latest stable releases of: Chrome, Firefox, Safari, Microsoft Edge.  
- Operating systems: Windows 10/11, macOS (latest). 

### Features To Be Tested (key flows)
- Valid weight and height input and successful BMI calculation.  
- Correct category labeling for BMI ranges:  
  - Underweight: BMI < 18.5  
  - Normal: 18.5 ≤ BMI ≤ 24.9  
  - Overweight: 25.0 ≤ BMI ≤ 29.9  
  - Obese: BMI ≥ 30.0  
- Robust handling of invalid input (non-numeric, out-of-range, empty).  
- UI behavior for reset/clear action.  
- Accessibility basics: labels, keyboard navigation, and alt text where applicable. 

### Features Not To Be Tested
- Persistent storage or user history features (the app does not save data).  
- Authentication or account features.  
- Native mobile app behaviors (this plan covers browser web app only).  
- Print/export functionality and multi-language support (out of scope for this phase). 

### Approach
- Manual test execution with documented test cases covering functional, boundary, negative, and compatibility scenarios.  
- Boundary Value Analysis for weight and height ranges.  
- Cross-browser testing on listed browsers and OS.  
- Defect logging with reproduction steps and screenshots; retest after fix. 

### Pass / Fail Criteria
- BMI computation must be 100% accurate according to the standard formula for the tested input set.  
- Classification must match the BMI range definitions.  
- No critical defects may exist that block the ability to calculate BMI.  
- Acceptance benchmark: **≥ 95% test-case pass rate** and zero unresolved blocking defects. 

### Suspension Criteria
Testing must be suspended if:  
- The application crashes or becomes unresponsive.  
- Core input fields fail to accept valid data.  
- A critical browser compatibility issue prevents basic calculation. 

### Test Deliverables
- Test cases and test data.  
- Test execution results (pass/fail/skip).  
- Bug reports with screenshots and reproduction steps.  
- Final test summary and release recommendation. 

### Testing Tasks & Responsibilities
- Prepare and review test cases.  
- Configure test environments and sample data.  
- Execute tests and log defects.  
- Test manager coordinates progress, developers fix critical defects, and testers verify fixes. 

### Environment & Tools
- Browsers listed above; test devices/VMs for Windows and macOS.  
- Test case management: spreadsheets or a simple ticketing system (e.g., GitHub Issues, JIRA).  
- Screenshot tool and basic verification calculator (for manual verification). 

### Staffing & Training
- 2–3 testers with basic web testing skills and understanding of BMI rules.  
- A short briefing on BMI calculation and classification prior to testing. 

### Schedule
- Total schedule: 2 weeks before release.  
  - Week 1: test execution (first pass)  
  - Week 2: defect fixes, retest, and final sign-off. 

### Risks & Mitigations
- Risk: Misunderstanding of BMI ranges → Mitigation: a domain primer and sample calculations for testers.  
- Risk: Missing builds or unstable environment → Mitigation: clear escalation and backup test environment. 

### Example Test Cases (sample table)

| ID | Test case | Input (weight kg / height cm) | Expected BMI | Expected Category |
|---:|---|---:|---:|---|
| TC-01 | Typical normal case | 68 / 175 | 22.2 | Normal |
| TC-02 | Underweight boundary | 50 / 175 | 16.3 | Underweight |
| TC-03 | Overweight boundary | 85 / 175 | 27.8 | Overweight |
| TC-04 | Obese boundary | 95 / 165 | 34.9 | Obese |
| TC-05 | Height as zero (invalid) | 70 / 0 | error / validation | Error message |
| TC-06 | Non-numeric weight | "abc" / 170 | validation | Error message |
| TC-07 | Reset functionality | fill fields → Reset | fields cleared | fields empty |

(Use these as templates — add more negative and boundary cases to reach comprehensive coverage.) 

### Approvals
After test completion, Test Manager and Project Manager must sign off that acceptance criteria are met before production deployment. 
