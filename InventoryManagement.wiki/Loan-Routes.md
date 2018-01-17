# Loan (/loans)

## **POST /loans**

Slim framework directs the request to create() in src/Controller/LoanController.php.

create() calls createLoan() in src/Core/CoreService.php (line 32 in LoanController.php), which then calls createLoan() in src/Core/DAO.php (line 238 CoreService.php).

**Input:**

All fields must be presented.

    {
    	"username" : "filoseta",
    	"equipments" : [
    		"58fbd1477f8b9ac357b09640"
    	],
        "is_return" : false,
        "due_date" : "2017-05-01 23:59:59"
    }

**Output:**

    {
      "ok": true,
      "msg": "Successfully created loan.",
      "loan": {
        "_id": {
          "$id": "58fbeff87f8b9ac557b09644"
        },
        "username": "filoseta",
        "equipments": [
          {
            "_id": {
              "$id": "58fbd1477f8b9ac357b09640"
            },
            "department_tag": "math-12342",
            "gt_tag": null,
            "equipment_type_name": "laptop",
            "comments": "some comment",
            "attributes": [
              {
                "_id": {
                  "$id": "58fbd1477f8b9ac357b09641"
                },
                "name": "serial_number",
                "value": "DS6AF5DS4F6-454645DSF",
                "equipment_type_attribute_id": {
                  "$id": "58fbd1337f8b9ac257b09643"
                },
                "equipment_type_id": {
                  "$id": "58fbd1337f8b9ac257b09642"
                },
                "equipment_id": {
                  "$id": "58fbd1477f8b9ac357b09640"
                },
                "logs": [
                  {
                    "_id": {
                      "$id": "58fbd1477f8b9ac357b09642"
                    },
                    "reference_id": {
                      "$id": "58fbd1477f8b9ac357b09641"
                    },
                    "document_type": "equipment_attribute",
                    "action_type": "create",
                    "timestamp": "2017-04-22 17:55:19",
                    "action_by": "some_user",
                    "action_via": "hard coded web",
                    "changes": []
                  }
                ]
              },
              {
                "_id": {
                  "$id": "58fbd1477f8b9ac357b09643"
                },
                "name": "serial_number1",
                "value": "DS6AF5DS4F6-454645DSF",
                "equipment_type_attribute_id": {
                  "$id": "58fbd1337f8b9ac257b09645"
                },
                "equipment_type_id": {
                  "$id": "58fbd1337f8b9ac257b09642"
                },
                "equipment_id": {
                  "$id": "58fbd1477f8b9ac357b09640"
                },
                "logs": [
                  {
                    "_id": {
                      "$id": "58fbd1477f8b9ac357b09644"
                    },
                    "reference_id": {
                      "$id": "58fbd1477f8b9ac357b09643"
                    },
                    "document_type": "equipment_attribute",
                    "action_type": "create",
                    "timestamp": "2017-04-22 17:55:19",
                    "action_by": "some_user",
                    "action_via": "hard coded web",
                    "changes": []
                  }
                ]
              }
            ],
            "status": "loaned",
            "loaned_to": "filoseta",
            "equipment_type_id": {
              "$id": "58fbd1337f8b9ac257b09642"
            },
            "created_on": "2017-04-22 17:55:19",
            "last_updated": "2017-04-22 20:06:16",
            "logs": [
              {
                "_id": {
                  "$id": "58fbd1477f8b9ac357b09645"
                },
                "reference_id": {
                  "$id": "58fbd1477f8b9ac357b09640"
                },
                "document_type": "equipment",
                "action_type": "create",
                "timestamp": "2017-04-22 17:55:19",
                "action_by": "some_user",
                "action_via": "hard coded web",
                "changes": []
              },
              {
                "_id": {
                  "$id": "58fbeff87f8b9ac557b09646"
                },
                "reference_id": {
                  "$id": "58fbd1477f8b9ac357b09640"
                },
                "document_type": "equipment",
                "action_type": "edit",
                "timestamp": "2017-04-22 20:06:16",
                "action_by": "some_user",
                "action_via": "hard coded web",
                "changes": [
                  {
                    "field_name": "loaned_to",
                    "old_value": null,
                    "new_value": "filoseta"
                  },
                  {
                    "field_name": "status",
                    "old_value": "inventory",
                    "new_value": "loaned"
                  }
                ]
              }
            ]
          }
        ],
        "due_date": "2017-05-01 23:59:59",
        "loaned_date": "2017-04-22 20:06:16",
        "is_returned": false,
        "logs": [
          {
            "_id": {
              "$id": "58fbeff87f8b9ac557b09647"
            },
            "reference_id": {
              "$id": "58fbeff87f8b9ac557b09644"
            },
            "document_type": "loan",
            "action_type": "create",
            "timestamp": "2017-04-22 20:06:16",
            "action_by": "hardcodedweb",
            "action_via": "hardcodedweb",
            "changes": []
          }
        ]
      }
    }

## **GET /loans/?field=value**
Slim framework directs the request to get() in src/Controller/LoanController.php

get() calls getLoan() in src/Core/CoreService.php (line 70 in LoanController.php), which then calls getLoan() in src/Core/DAO.php (line 232 in CoreService.php).

**Input:** URL query parameters is under the format: /?field=value. Spaces are denoted with underscores (_) and additional key, value parameters are separated by &. If URL query parameter is empty, this route performs “Get all”.

This API searches for the exact match and does not support nested search or conditional search.
'searchField' can be "username".

**Output:** JSON document with following keys: “ok” - boolean, “msg” - string (message), “n” - integer (number of loans), “loans” - array of Loan documents.

## **PUT /loans**

Slim framework directs the request to update() in src/Controller/LoanController.php

update() calls updateLoan() in src/Core/CoreService.php (line 53 in LoanController.php), which then calls updateLoan(), addEquipmentToLoan(), removeEquipmentFromLoan() in src/Core/DAO.php (line 276-293 in CoreService.php) based on the request JSON.

**Input:**

"_id" must be presented.
In "update_loan", setting "is_returned" to true updates equipments' status and loaned_to fields.

Sample request JSON:

    {
        "_id" : "58fbbbf07f8b9ac357b09638",

        "update_loan" : {
            "loaned_date" : "2017-01-01 00:00:00",
            "due_date" : "2017-01-01 00:00:00",
            "is_returned" : true
        },

        "add_equipments" : [
            "58fbbbf07f8b9ac357b09638"
        ],

        "remove_equipments" : [
            "58fbbbf07f8b9ac357b09638"
        ]
    }

**Output:**
JSON document with following keys: “ok” - boolean, “msg” - string (message), “loan” - updated Loan document.

## **DELETE/loans**

Slim framework directs the request to delete() in src/Controller/LoanController.php

delete() calls deleteLoan() in src/Core/CoreService.php (line 99 in EquipmentTypeController.php), which then calls  deleteLoan() in src/Core/DAO.php (line 632 in CoreService.php).

**Input:**

Bulk delete is not supported. '_id' must be present in the request JSON.

Sample request JSON:

    {
        "_id" : "58fbbbf07f8b9ac357b09638"
    }

**Output:**

JSON document with following keys: “ok” - boolean, “msg” - string (message)
