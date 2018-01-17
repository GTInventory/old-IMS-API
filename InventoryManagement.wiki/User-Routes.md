# # User (/users)

## **POST /users**

Slim framework directs the request to create() in src/Controller/UserController.php.

create() calls createUser() in src/Core/CoreService.php (line 79 in UserController.php), which then calls createUser() in src/Core/DAO.php (line 82 CoreService.php).

**Input:**

Sample request JSON:

    {
    	"username" : "filoseta",
    	"email" : "filoseta@gatech.edu"
    }


**Output:**

    {
      "ok": true,
      "msg": "Successfully created new user.",
      "user": {
        "_id": {
          "$id": "58fbb2897f8b9abf57b0963a"
        },
        "username": "filoseta",
        "email": "filoseta@gatech.edu",
        "current_loans": [],
        "past_loans": [],
        "logs": [
          {
            "_id": {
              "$id": "58fbb2897f8b9abf57b0963b"
            },
            "reference_id": {
              "$id": "58fbb2897f8b9abf57b0963a"
            },
            "document_type": "user",
            "action_type": "create",
            "timestamp": "2017-04-22 15:44:09",
            "action_by": "hardcodedweb",
            "action_via": "hardcodedweb",
            "changes": []
          }
        ]
      }
    }

## **GET /users/?field=value**

Slim framework directs the request to find() in src/Controller/UserController.php

find() calls getUser() in src/Core/CoreService.php (line 48 in UserController.php), which then calls getUser() in src/Core/DAO.php (line 115 in CoreService.php). 

**Input:** URL query parameters is under the format: /?field=value. Spaces are denoted with underscores (_) and additional key, value parameters are separated by &. If URL query parameter is empty, this route performs “Get all”.

This API searches for the exact match and does not support nested search or conditional search.
'searchField' can be "username" and "email".

**Output:** JSON document with following keys: “ok” - boolean, “msg” - string (message), “n” - integer (number of users), “users” - array of User documents.

## **PUT /users**

Slim framework directs the request to update() in src/Controller/UserController.php

update() calls updateUser() in src/Core/CoreService.php (line 100 in UserController.php), which then calls updateUserHelper() in CoreService.php (line 142 in CoreService.php).

updateUserHelper() calls updateUser(), addCurrentLoanToUser(), addPastLoanToUser(), removeCurrentLoanFromUser(), removePastLoanFromUser() in src/Core/DAO.php based on the request JSON. (line 151-184 in CoreService.php)

**Input:**

Either '_id' or 'username' must be present in the request JSON as an identifier.

    {
        "_id" : "58e81cc37f8b9ac4571ef7c6",
        "username" : "filoseta",

        "edit_user" :
        {
            "email" : "filoseta@gmail.com"
        },

        "add_current_loans" : [
            "58e81cc37f8b9ac4571ef7c6"
        ],

        "add_past_loans" : [
            "58e81cc37f8b9ac4571ef7c6"
        ],

        "remove_current_loans" : [
            "58e81cc37f8b9ac4571ef7c6"
        ],

        "remove_past_loans" : [
            "58e81cc37f8b9ac4571ef7c6"
        ]
    }

**Output:**
JSON document with following keys: “ok” - boolean, “msg” - string (message), “user” - updated User document.

## **DELETE/users**

Slim framework directs the request to delete() in src/Controller/UserController.php

delete() calls deleteUser() in src/Core/CoreService.php (line 121 in UserController.php), which then calls removeUser() in src/Core/DAO.php (line 205 in CoreService.php).

**Input:**

One of the following identifiers must be presented.

    {
    	"_id" : "58e81cc37f8b9ac4571ef7c6",
    	"username" : "filoseta"
    }

**Output:**

JSON document with following keys: “ok” - boolean, “msg” - string (message)
