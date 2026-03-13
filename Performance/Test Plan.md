

Thread Group Configuration
The test execution is controlled using a Thread Group

⚙️ Configuration
Number of Threads (Users): 1
Ramp-Up Period: 1 second
Loop Count: 1
On Sample Error: Continue
Same User on Next Iteration: Enabled
Purpose The Thread Group simulates a single virtual user executing the workflow once This configuration focuses on validating the end to end API workflow and token based authentication, rather than generating heavy load.

⚡Workflow Steps
1️ Visit Registration Page
Sampler: HTTPS GET /signup
Assertion: Response code = 200
Purpose: Verify signup page is reachable.
2️ Register New User
Sampler: HTTPS POST /api/v1/users/register
Payload:
json { "email": "tester${RandomNumber}@examble.com", "password": "Test@#123", "firstName": "test", "lastName": "test" }

Headers
text content-type: application/json

Assertions
Status Code = 201
Response body contains access_token
Random Variable
text RandomNumber (1–1000000) Used to generate a unique email address for each test execution.

JSON PostProcessor
text Extract access_token → ${Token} Purpose Validate successful user registration and obtain the authentication token for subsequent API requests.

3️ Get Tasks API
Sampler
text HTTPS GET /api/v1/tasks

Headers
text authorization: Bearer ${Token}

Assertions
Status Code = 200
Response body contains tasks Purpose Verify that the API correctly returns the task list for the authenticated user.
4️ Add Todo Task
Sampler
text HTTPS POST /api/v1/tasks

Payload
json { "item": "learn jmeter", "isCompleted": false }

Headers
text authorization: Bearer ${Token} content-type: application/json

Assertions
Status Code = 201
Response body contains learn jmeter Purpose Validate the successful creation of a new todo task.
5️ Reuse Get Tasks API
Element
text Module Controller Calls the Get Tasks API steps again Purpose Verify that the task list has been updated after adding the new task.

6️ Results Visualization
Listener
text View Results Tree Purpose

Inspect request and response details
Debug API requests
Verify that all assertions pass successfully
