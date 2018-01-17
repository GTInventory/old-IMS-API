# Equipment (/equipments)

Equipment document is simply a data holder that is depended on Equipment Type document.
Each attribute values in Equipment document is checked against Equipment Type Attribute's fields.

## **POST /equipments**

Slim framework directs the request to create() in src/Controller/EquipmentController.php.

create() calls createEquipment() in src/Core/CoreService.php (line 139 in EquipmentController.php), which then calls createEquipment() in src/Core/DAO.php (line 322 CoreService.php).

**Input:**

Unique identifiers are "department_tag", "gt_tag", and "_id". "_id" is set by the system and used internally.
Attribute's name and value are checked by the system using Equipment Type Attribute information.

Sample request JSON:

    {
    	"department_tag" : "math-12342",
    	"gt_tag" : null,
    	"equipment_type_name" : "laptop",
    	"comments" : "some comment",

    	"attributes" : [
    		{
    			"name" : "serial_number",
    			"value" : "DS6AF5DS4F6-454645DSF"
    		},
    		{
    			"name" : "serial_number1",
    			"value" : "DS6AF5DS4F6-454645DSF"
    		}
    	]
    }

**Output:**

    {
      "ok": true,
      "message": "Successfully created Equipment 'math-12342'.",
      "equipment": {
        "_id": {
          "$id": "58fbaa6e7f8b9ac057b0963c"
        },
        "department_tag": "math-12342",
        "gt_tag": null,
        "equipment_type_name": "laptop",
        "comments": "some comment",
        "attributes": [
          {
            "_id": {
              "$id": "58fbaa6e7f8b9ac057b0963d"
            },
            "name": "serial_number",
            "value": "DS6AF5DS4F6-454645DSF",
            "equipment_type_attribute_id": {
              "$id": "58fbaa697f8b9ac457b09637"
            },
            "equipment_type_id": {
              "$id": "58fbaa697f8b9ac457b09636"
            },
            "equipment_id": {
              "$id": "58fbaa6e7f8b9ac057b0963c"
            },
            "logs": [
              {
                "_id": {
                  "$id": "58fbaa6e7f8b9ac057b0963e"
                },
                "reference_id": {
                  "$id": "58fbaa6e7f8b9ac057b0963d"
                },
                "document_type": "equipment_attribute",
                "action_type": "create",
                "timestamp": "2017-04-22 15:09:34",
                "action_by": "some_user",
                "action_via": "hard coded web",
                "changes": []
              }
            ]
          },
          {
            "_id": {
              "$id": "58fbaa6e7f8b9ac057b0963f"
            },
            "name": "serial_number1",
            "value": "DS6AF5DS4F6-454645DSF",
            "equipment_type_attribute_id": {
              "$id": "58fbaa697f8b9ac457b09639"
            },
            "equipment_type_id": {
              "$id": "58fbaa697f8b9ac457b09636"
            },
            "equipment_id": {
              "$id": "58fbaa6e7f8b9ac057b0963c"
            },
            "logs": [
              {
                "_id": {
                  "$id": "58fbaa6e7f8b9ac057b09640"
                },
                "reference_id": {
                  "$id": "58fbaa6e7f8b9ac057b0963f"
                },
                "document_type": "equipment_attribute",
                "action_type": "create",
                "timestamp": "2017-04-22 15:09:34",
                "action_by": "some_user",
                "action_via": "hard coded web",
                "changes": []
              }
            ]
          }
        ],
        "status": "inventory",
        "loaned_to": null,
        "equipment_type_id": {
          "$id": "58fbaa697f8b9ac457b09636"
        },
        "created_on": "2017-04-22 15:09:34",
        "last_updated": "2017-04-22 15:09:34",
        "logs": [
          {
            "_id": {
              "$id": "58fbaa6e7f8b9ac057b09641"
            },
            "reference_id": {
              "$id": "58fbaa6e7f8b9ac057b0963c"
            },
            "document_type": "equipment",
            "action_type": "create",
            "timestamp": "2017-04-22 15:09:34",
            "action_by": "some_user",
            "action_via": "hard coded web",
            "changes": []
          }
        ]
      }
    }

## **GET /equipments/?field=value**

Slim framework directs the request to find() in src/Controller/EquipmentController.php

find() calls getEquipment() in src/Core/CoreService.php (line 76 in EquipmentController.php), which then calls getEquipment() in src/Core/DAO.php (line 332 in CoreService.php).

**Input:** URL query parameters is under the format: /?field=value. Spaces are denoted with underscores (_) and additional key, value parameters are separated by &. If URL query parameter is empty, this route performs “Get all”.

**Output:** JSON document with following keys: “ok” - boolean, “msg” - string (message), “n” - integer (number of equipments), “equipments” - array of Equipment documents.

    {
        "ok : false,
        "msg" : "Equipment not found with given search criteria.",
        "equipments" : [],
        "n" : 0
    }

### **Issues**
This API searches for exact match and does not support nested search or conditional search.
'searchField' cannot be a field in 'equipment_attributes'.
'searchField' can be "department_tag", "gt_tag", "equipment_type_name", "status", "loaned_to", "created_on", "last_updated", "comment".

## **PUT /equipments**

Slim framework directs the request to updateOne() in src/Controller/EquipmentController.php

updateOne() calls updateEquipment() in src/Core/CoreService.php (line 178 in EquipmentController.php), which then calls updateEquipment(), updateEquipmentAttribute(), addEquipmentAttribute(), removeEquipmentAttribute() in src/Core/DAO.php (line 396-419 in CoreService.php) based on the request JSON.

**Input:**

At least one identifier must be present in the request JSON.
Identifiers are "_id", "department_tag", and "gt_tag".

"update_equipment" - Updates fields of Equipment document.
"department_tag", "gt_tag", "status", "loaned_to", "comments" can be changed.
It is not possible to change equipment type of an Equipment.
"equipment_type_name" will be changed by the system if Equipment Type's name is changed.

"update_equipment_attributes" - Either "_id" or "name" must be present.
"value" is only field that can be changed and this will be checked against Equipment Type Attribute information.

"add_equipment_attributes" - "name" and "value" must be presented and this will be checked against Equipment Type Attribute information.

"remove_equipment_attributes" - ID string of Equipment Type Attribute must be present. Required attributes cannot be deleted.

Sample request JSON:

    {
    	"_id" : "58e81cc37f8b9ac4571ef7c6",
        "department_tag" : "math-12342",
    	"gt_tag" : null,

    	"update_equipment" :
    	{
            "department_tag": "math-12342",
            "gt_tag": null,
            "status": "loaned",
            "loaned_to": "filoseta",
            "comments": "some comment"
    	},

    	"update_equipment_attributes" :
    	[
    		{
    			"_id" : "58e81cc37f8b9ac4571ef7c6",
    			"name" : "serial_number",
                "value" : "some other value"
    		}
    	],

    	"add_equipment_attributes" :
    	[
    		{
                "name" : "serial_number999",
                "value" : "some value"
    		}
    	],

    	"remove_equipment_attributes" :
    	[
    		"58e81cc37f8b9ac4571ef7c7"
    	]
    }


**Output:**
JSON document with following keys: “ok” - boolean, “msg” - string (message), “updated_equipment” - updated Equipment document.

## **DELETE /equipments**

Slim framework directs the request to delete() in src/Controller/EquipmentTypeController.php

delete() calls deleteEquipment() in src/Core/CoreService.php (line 199 in EquipmentController.php), which then calls  deleteEquipment() in src/Core/DAO.php (line 480 in CoreService.php).

**Input:**

Bulk delete is not supported. One of the following unique identifiers must be presented in the request JSON.

Unique identifiers:
'_id', 'department_tag', 'gt_tag'

Sample request JSON:

    {
        "_id" : "58e81cc37f8b9ac4571ef7c6",
        "department_tag" : "math-1234",
        "gt_tag" : "gttag1234"
    }

**Output:**

JSON document with following keys: “ok” - boolean, “msg” - string (message)
