# APIs and Streamlit

## Original Class Notes

### RPA

1. PyAutoGUI —> For operating Desktop Apps
2. Playwright —> For operating / automating our tasks on browser
3. PyAuto GUI —> Automate our task using lap mouse / keyboard —> its Python library
4. PyAutoGUI Functionalities —> Mouse control, keyboard control, multiple window control, screenshot

### API

API —> Communication between two servers

Types:
1. REST API
2. Graph QL —> Meta is using this
3. SOAP
4. RPC
5. Web socket

### REST API

method on REST API
A. POST —> Creating the new entry
B. GET —> getting the details from server
C. PUT —> modifying the details on server
D. DELETE —> deleting the record

4xx Client Errors
5xx Server Errors
2xx Success
3xx Redirection

POSTMAN tool used to test our API’s

### Streamlit

Streamlit is used for building and sharing interactive web applications and dashboards entirely in Python, eliminating the need for frontend web development skills like HTML, CSS, or JavaScript.

Streamlit is easy frontend creating app without using HTML, CSS

## Additional Study Details

### API mental model

An API defines how one software component communicates with another. A request commonly contains a method, URL/path, headers, optional query parameters, and a body; the response commonly contains a status code, headers, and a body such as JSON.

### HTTP methods

GET is generally used to retrieve data, POST to create or trigger an operation, PUT to replace/update a resource, PATCH for partial updates, and DELETE to remove a resource. Exact semantics depend on the API design.

### Status codes

2xx indicates successful processing, 3xx redirection, 4xx client-side/request problems, and 5xx server-side failures. Know common examples such as 200, 201, 400, 401, 403, 404, 409, 429, and 500.

### API security

Common mechanisms include API keys, bearer tokens, OAuth 2.0, signed requests, and mutual TLS. Authentication answers 'who are you?'; authorization answers 'what are you allowed to do?'.

### RPA and browser automation

PyAutoGUI is useful for desktop-level mouse/keyboard interaction. Playwright automates browsers and is commonly used for testing and browser workflows. API automation is usually more stable than UI automation when a supported API exists.

### Streamlit

Streamlit is useful for quickly turning Python logic, data workflows, and AI applications into interactive web apps. It is especially useful for prototypes and internal tools.
