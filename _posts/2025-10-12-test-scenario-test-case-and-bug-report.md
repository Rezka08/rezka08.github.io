---
title: "Test Scenario, Test Case, and Bug Report"
date: 2025-10-12 12:00:00
categories: [Software Testing and Quality Assurance]
tags: [STQA]
image: /assets/images/STQA/test-scenario-test-case-and-bug-report.png
---

# Test Scenario, Test Case, and Bug Report

This document translates and consolidates the group's material on test scenarios, test cases, and bug reporting into a reusable English reference. It includes templates, a set of concrete test scenarios and test cases for a BMI application, an example bug report, severity/priority guidelines, and best practices to avoid bugs. :contentReference[oaicite:1]{index=1}

---

## Overview — Definitions

- **Test Scenario** — A high-level description of what should be tested; it describes a functionality or user story to be validated.  
- **Test Case** — A detailed, step-by-step instruction set for executing a test, including inputs, preconditions, expected results, and pass/fail criteria.  
- **Bug Report** — A structured report describing a defect found during testing, including steps to reproduce, actual vs expected results, environment, severity, and priority.

---

## Simple Templates

### Test Scenario (simple)

| Field | Description |
|---|---|
| ID Scenario | Unique scenario identifier (e.g., TS001) |
| Description | Short summary of the scenario to be tested |
| Module / Feature | The module or feature under test |

### Test Case (simple)

| Field | Description |
|---|---|
| ID Test Case | Unique test case ID (e.g., TC001) |
| Description | Brief description of the test |
| Precondition | State of the system before test begins |
| Test Steps | Step-by-step instructions to perform the test |
| Test Data | Specific input data to use |
| Expected Result | What should occur if the system is correct |
| Actual Result | Observed result after execution (filled during testing) |
| Status | Pass / Fail (filled during testing) |

---

## Example: BMI Application — Test Scenarios

**ID** | **Description** | **Module / Feature**
---:|---|---
TS001 | Verify slider input behavior | Weight and height sliders
TS002 | Verify BMI calculation and classification | BMI calculation & classification
TS003 | Verify BMI history saving | BMI history (storage/display)

*(These scenarios are derived from the project test plan.)* :contentReference[oaicite:2]{index=2}

---

## Example: BMI Application — Test Cases

> Note: BMI formula is `BMI = weight(kg) / (height(m))^2`. Expected BMI values below are **rounded to 2 decimal places**.

| ID | Test Case | Precondition | Test Steps | Test Data | Expected Result |
|---:|---|---|---|---:|---|
| TC001 | Verify weight slider updates the weight label | App is open | 1. Move weight slider to a value. 2. Observe weight label/field. | Slider → 60 kg | Weight label displays `60 kg` matching slider position. |
| TC002 | Verify height slider updates the height label | App is open | 1. Move height slider to a value. 2. Observe height label/field. | Slider → 170 cm | Height label displays `170 cm` matching slider position. |
| TC003 | Verify BMI calculation uses standard formula | App is open | 1. Set height → 170 cm. 2. Set weight → 65 kg. 3. Press Calculate. | Height = 170 cm, Weight = 65 kg | BMI ≈ **22.49** → correct calculation shown. |
| TC004 | Verify BMI classification: Underweight | App is open | 1. Set height → 170 cm. 2. Set weight → 45 kg. 3. Press Calculate. | Height = 170 cm, Weight = 45 kg | BMI ≈ **15.57** → category `Underweight`. |
| TC005 | Verify BMI classification: Normal | App is open | 1. Set height → 165 cm. 2. Set weight → 60 kg. 3. Press Calculate. | Height = 165 cm, Weight = 60 kg | BMI ≈ **22.04** → category `Normal`. |
| TC006 | Verify BMI classification: Overweight | App is open | 1. Set height → 170 cm. 2. Set weight → 75 kg. 3. Press Calculate. | Height = 170 cm, Weight = 75 kg | BMI ≈ **25.95** → category `Overweight`. |
| TC007 | Verify BMI classification: Obese | App is open | 1. Set height → 165 cm. 2. Set weight → 90 kg. 3. Press Calculate. | Height = 165 cm, Weight = 90 kg | BMI ≈ **33.06** → category `Obese`. |
| TC008 | Verify saving latest BMI to History | App is open | 1. Set weight & height, 2. Press Save History. 3. Open History page. | Height = 170 cm, Weight = 65 kg | User is navigated to History and latest BMI entry is saved and visible. |
| TC009 | Verify saving multiple history entries without loss | App is open and history has entries | 1. Save multiple BMI results sequentially. 2. Open History page. | Series of saved BMI entries | All entries are preserved in order; no data loss. |

**Notes:**  
- I corrected several expected BMI computations to match the standard formula and rounded to two decimals. The original slides had a couple of arithmetic inconsistencies; the corrected values above are used as authoritative expected results during testing. :contentReference[oaicite:3]{index=3}

---

## Bug Report — Formal Template & Example

### Bug Report Template (fields)
- **Bug ID:** unique id (e.g., BMI-001)  
- **Title:** short descriptive title  
- **Reporter:** name / role (e.g., SQA)  
- **Assignee:** developer or team responsible  
- **Severity:** Low / Minor / Major / Critical (impact on functionality)  
- **Priority:** P1 (Urgent) / P2 (High) / P3 (Medium) / P4 (Low)  
- **Environment:** OS / Browser / App version / Build number / Device  
- **Steps to Reproduce:** numbered steps to reproduce the bug reliably  
- **Actual Result:** observed behavior (attach screenshots / logs)  
- **Expected Result:** what should happen according to requirements  
- **Build / Version:** application build where bug was observed  
- **Reported On:** date  
- **Status:** Open / In Progress / Resolved / Closed

### Example Bug Report (from BMI scenario)
- **Bug ID:** BMI-001  
- **Title:** Incorrect BMI result for Weight = 60 kg and Height = 170 cm  
- **Reporter:** SQA  
- **Assignee:** Developer  
- **Severity:** Major (High) — classification logic or calculation is wrong but app not crashing  
- **Priority:** P2 — High  
- **Environment:** Android (Device model), App Version 1.0.0, Build 1.0.0  
- **Steps to Reproduce:**  
  1. Open the BMI application.  
  2. Enter Weight = `60`.  
  3. Enter Height = `170`.  
  4. Tap the `Calculate` button.  
- **Actual Result:** BMI displayed = `12.50`.  
- **Expected Result:** BMI should be ≈ **20.76** (60 / 1.70^2 = 20.76).  
- **Reported On:** 2025-08-31  
- **Status:** Open

> Tip: Attach a screenshot of the incorrect output and include logs or console output when available.

---

## Severity & Priority — Quick Guide

**Severity (impact):**
- **Low:** Cosmetic or minor issue; no feature impact.  
- **Minor / Medium:** Usability annoyance; does not affect core functionality.  
- **Major / High:** Core functionality broken but app still partially usable.  
- **Critical:** System crash, data loss, or complete feature failure.

**Priority (fix order):**
- **P1 (Urgent / Critical):** Fix immediately before next release.  
- **P2 (High):** High importance; fix soon.  
- **P3 (Medium):** Schedule for a subsequent release.  
- **P4 (Low):** Cosmetic / non-urgent.

---

## How to Avoid Bugs — Best Practices (summary)
1. **Understand requirements** clearly before development.  
2. **Unit testing**: catch defects early with automated unit tests.  
3. **Code review**: peer review to find logical or stylistic issues.  
4. **Test planning**: prepare a comprehensive test plan before execution.  
5. **Automation**: automate repetitive tests (unit/integration/visual).  
6. **Team collaboration**: close feedback loops between developers and testers.

---

[View Group 4 Presentation Slides](https://drive.google.com/file/d/1er2wwoA69y6OFqgGZCR4S9YeQEx0rM6z/view?usp=sharing)