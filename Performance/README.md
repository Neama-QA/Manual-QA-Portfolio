# 🏃‍♂️ TodoPerformanceProject – JMeter Test Plan

## 📋 Overview
**Tool:** Apache JMeter 5.6.3  
**Target App:** [https://todo.qacart.com](https://todo.qacart.com)  

**Goal:**  
Simulate an end-to-end workflow on the Todo API to validate functionality and token-based authentication.

---

# ⚡ Workflow Steps

## 1️ Visit Registration Page
- **Sampler:** HTTPS GET `/signup`  
- **Assertion:** Response code = 200 ✅  
- **Purpose:** Verify signup page is reachable.
---


## 2️ Register New User
- **Sampler:** HTTPS POST `/api/v1/users/register`  
- **Payload:**  
```json
{
  "email": "tester${RandomNumber}@examble.com",
  "password": "Test@#123",
  "firstName": "test",
  "lastName": "test"
}
```
- ### Headers
```text
content-type: application/json
```
- ### Assertions
 * 1  Status Code = **201**
 * 2 Response body contains **access_token**

- ### Random Variable
```text
RandomNumber (1–1000000)
```
Used to generate a **unique email address** for each test execution.

- ### JSON PostProcessor
```text
Extract access_token → ${Token}
```
Purpose
Validate successful user registration and obtain the **authentication token** for subsequent API requests.


## 3️ Get Tasks API
- ### Sampler
```text
HTTPS GET /api/v1/tasks
```
- ### Headers
```text
authorization: Bearer ${Token}
```
- ### Assertions
* Status Code = **200**
* Response body contains **tasks**
Purpose
Verify that the API correctly **returns the task list for the authenticated user**.
---


## 4️ Add Todo Task
- ### Sampler
```text
HTTPS POST /api/v1/tasks
```
- ### Payload
```json
{
  "item": "learn jmeter",
  "isCompleted": false
}
```
- ### Headers
```text
authorization: Bearer ${Token}
content-type: application/json
```
- ### Assertions
* Status Code = **201**
* Response body contains **learn jmeter**
 Purpose
Validate the successful **creation of a new todo task**.
---

## 5️ Reuse Get Tasks API

- ### Element
```text
Module Controller
```
Calls the **Get Tasks API steps again**.
## Purpose
Verify that the **task list has been updated after adding the new task**.

---

## 6️ Results Visualization
- ### Listener
```text
View Results Tree
```
## Purpose

* Inspect request and response details
* Debug API requests
* Verify that all assertions pass successfully

---

# 🌟 Key Points

* End-to-end **API workflow testing**
* **Dynamic user registration**
* **Token extraction and reuse** for authenticated requests
* **Reusable Module Controller** for the Tasks API
* Clear **assertions for status codes and response content**

---

# 📁 File Contents

| File / Folder                 | Description                           |
| ----------------------------- | ------------------------------------- |
| `TodoPerformanceTestPlan.jmx` | Full Apache JMeter test plan          |
| `README.md`                   | Project documentation                 |
| `Screenshots/`                | Optional screenshots from Result Tree |
| `Notes/`                      | Optional detailed documentation       |

---

# 🚀 How to Run

### Step 1

Open the test plan file in **Apache JMeter**

```text
TodoPerformanceTestPlan.jmx
```

### Step 2

Run the **Thread Group**

### Step 3

Inspect execution results using

```text
View Results Tree
```
