# Log (/logs)

## **GET /logs/?params**

Slim framework directs the request to find() in src/Controller/LogController.php

find() calls getLog() in src/Core/CoreService.php (line 28 in LogController.php), which then calls getLog() in src/Core/DAO.php (line 40 in CoreService.php).

**Input:** URL query parameters is under the format: /?field=value. Spaces are denoted with underscores (_) and additional key, value parameters are separated by &. If URL query parameter is empty, this route performs “Get all”.

**Output:** JSON document with following keys: “ok” - boolean, “msg” - string (message), “n” - integer (number of logs), “logs” - array of Log documents.

* **Create, Update, Delete operations are handled by the backend.