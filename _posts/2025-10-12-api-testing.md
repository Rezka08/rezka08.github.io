---
title: "API Testing"
date: 2025-10-12 14:00:00
categories: [Software Testing and Quality Assurance]
tags: [STQA]
image: /assets/images/STQA/api-testing.png
---

# API Testing — Concepts, Tools, and Practical Examples

This article presents a compact, practical guide to API testing: what it is, why it matters, common tools (Postman, SoapUI, Newman, RestAssured, JMeter), the anatomy of requests/responses, example requests (GET/POST), automation techniques, security checks, and a sample test-case checklist.

---

## What is API Testing?

API testing verifies the behavior, reliability, performance, and security of Application Programming Interfaces (APIs). Unlike UI testing, API testing interacts directly with the application's business logic layer via HTTP (or other protocols) and validates responses, headers, status codes, and data formats.

**Why test APIs?**
- Ensures endpoints behave according to specification.  
- Catches defects early in the integration layer.  
- Enables fast, headless validation (no UI required).  
- Provides a foundation for reliable integrations between systems.

---

## Key benefits of API testing

- **Speed & Coverage:** tests are fast and can exercise many edge cases.  
- **Automation-friendly:** easy to integrate into CI pipelines.  
- **Security & Stability checks:** identify broken contracts, auth issues, and performance bottlenecks.  
- **Testability of business logic** independent of frontend UI.

---

## Common API testing tools

- **Postman** — user-friendly, collections, environments, test scripting, automation with Newman.  
- **SoapUI** — good for SOAP and advanced REST testing (security, data-driven scenarios).  
- **Newman** — CLI runner for Postman collections (CI integration).  
- **RestAssured** (Java) — fluent Java library for REST API tests.  
- **JMeter** — performance/load testing for APIs.  
- **Insomnia** — lightweight REST client with environment support.  
- **curl** — quick command-line requests for debugging.

---

## Anatomy of a request & response

**Request (common parts)**  
- **HTTP Method (Verb):** GET, POST, PUT, PATCH, DELETE, OPTIONS.  
- **URL / Endpoint**: `/api/v1/users/123`.  
- **Headers:** `Content-Type`, `Authorization`, `Accept`.  
- **Query params / Path params:** `?page=2`, `/users/{id}`.  
- **Body (for POST/PUT/PATCH):** JSON, XML, form data.

**Response (common parts)**  
- **Status code:** 200, 201, 400, 401, 404, 500 — indicates result of request.  
- **Headers**: caching, content-type, auth-related headers.  
- **Body**: JSON/XML payload containing data or error details.  
- **Timing**: response time for performance assertions.

[View Group 6 Presentation Slides](https://drive.google.com/file/d/1b5R0aV7jftn-94nSDdquKrhRcgBoj1v4/view?usp=sharing)

---

## Quick examples (curl + Postman test script)

### Example — GET (curl)

```bash
curl -i "https://api.example.com/v1/users/123" \
    -H "Accept: application/json" \
    -H "Authorization: Bearer <TOKEN>"
```

### Postman — simple assertions (in Tests tab, JavaScript)

```bash
pm.test("Status code is 201", function () {
  pm.response.to.have.status(201);
});

# response time
pm.test("Response time less than 500ms", function () {
  pm.expect(pm.response.responseTime).to.be.below(500);
});

# JSON body keys and values
const json = pm.response.json();
pm.test("id exists", () => pm.expect(json).to.have.property("id"));
pm.test("name is Rezka", () => pm.expect(json.name).to.eql("Rezka"));
```

#### Run Postman collections in CI using Newman
```bash
# install newman globally (one-time)
npm install -g newman

# run collection
newman run my_collection.json -e my_environment.json --reporters cli,html
```

### Types of API tests
- **Functional tests**: validate endpoints return expected results for valid inputs.
- **Negative tests**: invalid inputs, missing params, bad content-types, malformed JSON.
- **Contract tests**: validate response schema and data types (JSON Schema).
- **Security tests**: authentication/authorization, injection, rate-limiting checks.
- **Performance/load tests**: throughput, latency under load (JMeter / k6).
- **Integration tests**: how multiple services behave together.

### Example test cases / scenarios

|     ID | Scenario                    | Steps                         |                                     Expected |
| -----: | --------------------------- | ----------------------------- | -------------------------------------------: |
| API-01 | GET user — valid id         | GET /v1/users/123             |                   200 OK + correct user JSON |
| API-02 | GET user — not found        | GET /v1/users/999999          |                404 Not Found + error message |
| API-03 | Create user — valid payload | POST /v1/users {name,email}   | 201 Created + Location header + body with id |
| API-04 | Create user — missing email | POST /v1/users {name}         |         400 Bad Request + validation message |
| API-05 | Auth required               | GET /v1/private without token |                             401 Unauthorized |
| API-06 | Rate limiting               | Make 200 requests in 1s       |        429 Too Many Requests after threshold |

### Contract & schema validation

Use JSON Schema / OpenAPI (Swagger) to define expected request/response shapes. Validate responses against these schemas during tests to detect breaking changes early.

#### Example (Postman schema validation)

```java
const schema = {
  "type": "object",
  "required": ["id", "name", "email"],
  "properties": {
    "id": {"type":"integer"},
    "name": {"type":"string"},
    "email": {"type":"string", "format":"email"}
  }
};
pm.test("Schema is valid", function() {
  pm.response.to.have.jsonSchema(schema);
});
```

#### Security checks (quick list)
- Verify endpoints requiring auth return 401/403 when unauthorized.
- Test role-based access (user vs admin endpoints).
- Check for injection vulnerabilities (SQL, command) by sending crafted payloads.
- Ensure sensitive data not exposed in responses (passwords, tokens).
- Verify TLS and certificate validity for production endpoints.

#### Performance & load testing
- Use JMeter, k6, or Gatling to simulate load and measure latency/throughput.
- Define realistic scenarios for concurrent users and ramp-up times.
- Monitor server metrics (CPU, memory) while load tests run.
- Establish SLA thresholds (e.g., median response < 300ms, p95 < 1s).

#### Automating API tests in CI
1. Save tests as Postman collection (JSON) or write tests with a code-based framework (RestAssured, pytest + requests).
2. Run with Newman or test runner in CI (GitHub Actions, GitLab CI).
3. Fail the build on assertion failures and publish reports (HTML/JSON).
4. Use mock servers for predictable test data and to decouple from unstable dependencies.

### Appendix — Useful commands & snippets

#### Newman (run collection locally)
```bash
newman run my_collection.json -e my_environment.json --reporters cli,html --reporter-html-export newman-report.html
```

#### Basic curl for debugging
```bash
curl -X GET "https://api.example.com/v1/health" -i
```

#### Example: RestAssured (Java) quick test
```java
import io.restassured.RestAssured;
import org.junit.jupiter.api.Test;
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

class ApiTest {
  @Test
  void getUserReturns200() {
    RestAssured.baseURI = "https://api.example.com";
    given()
      .when().get("/v1/users/123")
      .then().statusCode(200).body("id", equalTo(123));
  }
}
```