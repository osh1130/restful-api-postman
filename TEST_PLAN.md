# 🧪 API Test Plan – Restful Booker BVT

**Author:** Vivi  
**Project:** Restful Booker API Postman Automation  
**API Documentation:** [https://restful-booker.herokuapp.com/apidoc/index.html](https://restful-booker.herokuapp.com/apidoc/index.html)

---

## 🎯 Objectives

- Verify the correctness and stability of the Restful Booker API through automated tests.
- Ensure all critical functionalities work across positive, negative, and edge scenarios.
- Support CI/CD integration for regression detection and delivery pipeline validation.

---

## 🔍 Scope

### ✅ Included

| Feature           | Endpoint(s)                             | Description                                |
|-------------------|------------------------------------------|--------------------------------------------|
| Health Check      | `GET /ping`                              | Ensure API is up                           |
| Authentication    | `POST /auth`                             | Validate correct and incorrect credentials |
| Search Bookings   | `GET /booking`, with filters             | Firstname, lastname, checkin, checkout     |
| Get Booking       | `GET /booking/:id`                       | Positive and invalid ID retrieval          |
| Create Booking    | `POST /booking`                          | Valid and invalid creation inputs          |
| Update Booking    | `PUT /booking/:id`, `PATCH /booking/:id` | Full and partial updates, with auth        |
| Delete Booking    | `DELETE /booking/:id`                    | Booking removal and verification           |

### ❌ Not Included (Currently)

- Performance testing (e.g., load, stress)
- Security penetration testing
- UI testing

---

## 🧠 Test Strategy

## 🧪 Test Suite Structure

The current collection is structured as a Basic Verification Test (BVT) suite and provides comprehensive coverage of all critical API endpoints. For improved flexibility and maintainability—especially in CI/CD pipelines—we recommend logically organizing the test cases into the following suites:

| Suite       | Description                                                           | Example Cases                        |
|-------------|------------------------------------------------------------------------|--------------------------------------|
| Smoke       | Minimal set to verify system is alive and functioning                 | `GET /ping`, `POST /booking`         |
| BVT         | Medium-weight suite to verify core API functionality                  | Create, update, delete, auth flows   |
| Regression  | Full test suite covering positive, negative, edge, and failure paths  | All endpoints, invalid data, etc.    |

Tests can be tagged using identifiers like @smoke, @bvt, or @regression directly in the request names or descriptions. These tags allow you to selectively execute specific subsets using:

Postman Collection Runner filters

Newman with folder selection or custom scripts

This structure enables fast feedback in early deployment stages, while still supporting thorough validation in later testing phases.


### Types of Testing

| Type                | Description                                                 |
|---------------------|-------------------------------------------------------------|
| Functional          | Ensures endpoint behavior matches specification             |
| Boundary/Edge Cases | Tests null, invalid inputs, missing fields, bad IDs         |
| Negative Testing    | Validates appropriate failure responses                     |
| Assertion Testing   | Confirms status codes, data content, and structure          |
| Isolated Data       | Uses pre-request scripts to dynamically create test data    |
| Schema Validation   | *(Planned)* Add JSON Schema assertions for response format  |

---

## 🧪 Test Case Design

- Each test is atomic and independent.
- Uses dynamic variables (`pm.variables`) for randomized values.
- Pre-request scripts generate consistent, realistic test data.
- Assertions ensure correct field values, structure, and failure handling.

---

## 🧪 Test Execution

### Manual

- Use Postman Collection Runner with environment file.

### Automated

```bash
newman run RestfulBookerBVT.postman_collection.json \
  -e RestfulBookerBVT.environment.json \
  --reporters cli,html \
  --reporter-html-export newman-report.html
```

### GitHub Actions CI

- GitHub workflow auto-executes tests on push/pull request.
- See `.github/workflows/postman-tests.yml`

---

## 🌍 Test Environments

The tests are designed to run across multiple environments depending on the stage of development:

| Environment  | `rb_url` Example                                | Purpose                                   |
|--------------|--------------------------------------------------|-------------------------------------------|
| Production   | `https://restful-booker.herokuapp.com`           | Public or staging environment for system-level verification |
| Local        | `http://localhost:3001`  | Local development environment for debugging |
| Mock         | `https://<your-mock-server>.mockapi.io`    | Used before backend implementation is complete |

**All other variables** (e.g., `firstname`, `lastname`, `checkin`, `checkout`) are dynamically generated in the pre-request script and do not need environment-specific values.


---

## 📈 Reporting

| Method           | Tool       | Notes                              |
|------------------|------------|------------------------------------|
| CLI              | Newman     | Console-based summary              |
| HTML             | Newman     | Human-readable report              |
| CI Logs          | GitHub     | Pass/fail output from workflows    |

---

## ✅ Ownership

- **Created by:** Vivi
- **GitHub Repository:** [Your GitHub Project Link]
- **API Reference:** [https://restful-booker.herokuapp.com/apidoc/index.html](https://restful-booker.herokuapp.com/apidoc/index.html)
