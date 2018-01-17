# Equipment Type (/equipmenttypes)

Equipment type document defines Equipment's attributes.
This enables the backend to have multiple Equipment Types and each Equipment Type can have the unlimited number of attributes while using fixed number of MongoDB collections (tables in RDBMS).

Equipment Type's attribute has fields to regulate Equipment Attribute's values.
This is explained in detail in POST request section.

## **POST /equipmenttypes**

Slim framework directs the request to create() in src/Controller/EquipmentTypeController.php.

create() calls createEquipmentType() in src/Core/CoreService.php (line 58 in EquipmentTypeController.php), which then calls createEquipmentType() in src/Core/DAO.php (line 509 CoreService.php).

**Input:**

Unique identifiers are "name" and "_id". "_id" is used internally.
Each Equipment Type Attribute in Equipment Type document must have unique name.

"required" - Forces Equipment to have an attribute.
"unique" - Enforces uniqueness of given Equipment Attribute. For example, laptop's serial number.
"data_type" - Not supported in current version. It is required to have 'data_type' field in request JSON but it is unused and unchecked.
"regex" - Enforces value of Equipment Attribute to follow regex format.
"enum" - true = set, false = unset. Enforces value of Equipment Attribute to be one of the 'enum_values' element.
"enum_values" - If enum is set, value of Equipment Attribute must be one of the 'enum_values' element.

"data_type" is unsupported due to current way of searching.
Nested search (searching for attribute's fields) and conditional search is not possible.
This is one of the milestone on next release.

Sample request JSON:

    {
      "name" : "laptop",
      "comments" : "some comments here.",
      "equipment_type_attributes": [
        {
          "name": "serial_number",
          "required": true,
          "unique": true,
          "data_type" : "string",
          "regex" : "some regex string",
          "help_comment" : "some comment",
          "enum" : false,
          "enum_values" : [
            "value1", "value2", "value3"
          ]
        },
        {
          "name": "serial_number1",
          "required": true,
          "unique": true,
          "data_type" : "string",
          "regex" : "some regex string",
          "help_comment" : "some comment",
          "enum" : false,
          "enum_values" : [
            "value1", "value2", "value3"
          ]
        }
      ]
    }

**Output:**

    {
      "ok": true,
      "message": "Successfully created EquipmentType 'laptop' !",
      "equipment_type": {
        "_id": {
          "$id": "58fba0747f8b9ac057b09636"
        },
        "name": "laptop",
        "comments": "some comments here.",
        "equipment_type_attributes": [
          {
            "_id": {
              "$id": "58fba0747f8b9ac057b09637"
            },
            "name": "serial_number",
            "required": true,
            "unique": true,
            "data_type": "string",
            "regex": "some regex string",
            "help_comment": "some comment",
            "enum": false,
            "enum_values": [
              "value1",
              "value2",
              "value3"
            ],
            "equipment_type_id": {
              "$id": "58fba0747f8b9ac057b09636"
            },
            "logs": [
              {
                "_id": {
                  "$id": "58fba0747f8b9ac057b09638"
                },
                "reference_id": {
                  "$id": "58fba0747f8b9ac057b09637"
                },
                "document_type": "equipment_type_attribute",
                "action_type": "create",
                "timestamp": "2017-04-22 14:27:00",
                "action_by": "some_user",
                "action_via": "hard coded web",
                "changes": []
              }
            ]
          },
          {
            "_id": {
              "$id": "58fba0747f8b9ac057b09639"
            },
            "name": "serial_number1",
            "required": true,
            "unique": true,
            "data_type": "string",
            "regex": "some regex string",
            "help_comment": "some comment",
            "enum": false,
            "enum_values": [
              "value1",
              "value2",
              "value3"
            ],
            "equipment_type_id": {
              "$id": "58fba0747f8b9ac057b09636"
            },
            "logs": [
              {
                "_id": {
                  "$id": "58fba0747f8b9ac057b0963a"
                },
                "reference_id": {
                  "$id": "58fba0747f8b9ac057b09639"
                },
                "document_type": "equipment_type_attribute",
                "action_type": "create",
                "timestamp": "2017-04-22 14:27:00",
                "action_by": "some_user",
                "action_via": "hard coded web",
                "changes": []
              }
            ]
          }
        ],
        "logs": [
          {
            "_id": {
              "$id": "58fba0747f8b9ac057b0963b"
            },
            "reference_id": {
              "$id": "58fba0747f8b9ac057b09636"
            },
            "document_type": "equipment_type",
            "action_type": "create",
            "timestamp": "2017-04-22 14:27:00",
            "action_by": "some_user",
            "action_via": "hard coded web",
            "changes": []
          }
        ]
      }
    }

## **GET /equipmenttypes/?field=value**

Slim framework directs the request to find() in src/Controller/EquipmentTypeController.php

find() calls getEquipmentType() in src/Core/CoreService.php (line 31 in EquipmentTypeController.php), which then calls getEquipmentType() in src/Core/DAO.php (line 519 in CoreService.php).

**Input:** URL query parameters is under the format: /?field=value. Spaces are denoted with underscores (_) and additional key, value parameters are separated by &. If URL query parameter is empty, this route performs “Get all”.

**Output:** JSON document with following keys: “ok” - boolean, “msg” - string (message), “n” - integer (number of equipment types), “equipment_types” - array of Equipment Type documents.

## **PUT /equipmenttypes**

Slim framework directs the request to updateOne() in src/Controller/EquipmentTypeController.php

updateOne() calls updateEquipmentType() in src/Core/CoreService.php (line 72 in EquipmentTypeController.php), which then calls updateEquipmentType(), updateEquipmentTypeAttribute(), addEquipmentTypeAttribute(), removeEquipmentTypeAttribute() in src/Core/DAO.php (line 566-589 in CoreService.php).

**Input:**

Either '_id' or 'name' must be present in the request JSON as an identifier.

'update_equipment_type' updates an Equipment Type document's fields.
This cannot be used to update Equipment Type Attribute.

'update_equipment_type_attributes' updates Equipment Type Attributes in an Equipment Type document.
'_id' is not update-able and must be presented.
All other fields are optional.

"unique" - Setting unique from false to true does not affect current Equipments until their next update.
Uniqueness check will be applied on the next update or new Equipments.

"data_type" - This is unsupported in current version.

"regex" - Just like "unique", changing this does not affect current Equipments until their next update.
Regular expression check will be applied to new Equipments or current Equipment's next update.

"enum" - Just like "unique", changing this does not affect current Equipments until their next update.
Enum check will be applied to new Equipments or current Equipment's next update.

'add_equipment_type_attributes' adds new Equipment Type Attribute in an Equipment Type document.
All fields are expected in the JSON. This won't affect current Equipments until their next update.

'remove_equipment_type_attributes' removes Equipment Type Attributes in an Equipment Type document.
MongoId strings are expected. Remove won't be allowed if there is an Equipment Attribute that has this Equipment Type Attribute.

Updating this document may not always work as expected since all Equipment documents are dependent
on this document. Please refer to the error message for details.

Sample request JSON:

    {
    	"_id" : "58e81cc37f8b9ac4571ef7c6",
        "name" : "laptop",

    	"update_equipment_type" :
    	{
    		"name" : "laptop2",
    		"comments" : "Changed laptop to laptop2."
    	},

    	"update_equipment_type_attributes" :
    	[
    		{
    			"_id" : "58e81cc37f8b9ac4571ef7c6",
    			"name" : "serial_number",
    			"required" : true,
    			"unique" : true,
    			"data_type" : "string",
    			"regex" : "some regex string",
    			"help_comment" : "some comment.",
    			"enum" : false,
    			"enum_values" : ["value1", "value2", "value3"]
    		}
    	],

    	"add_equipment_type_attributes" :
    	[
    		{
    			"name": "serial_number4",
    			"required": true,
    			"unique": true,
    			"data_type" : "string",
    			"regex" : "some regex string",
    			"help_comment" : "some comment",
    			"enum" : false,
    			"enum_values" : [
    				"value1", "value2", "value3"
    			]
    		}
    	],

    	"remove_equipment_type_attributes" :
    	[
    		"58e81cc37f8b9ac4571ef7c7"
    	]
    }

**Output:**
JSON document with following keys: “ok” - boolean, “msg” - string (message), “updated_equipment_type” - updated Equipment Type document.

## **DELETE/equipmenttypes**

Slim framework directs the request to delete() in src/Controller/EquipmentTypeController.php

delete() calls deleteEquipmentType() in src/Core/CoreService.php (line 94 in EquipmentTypeController.php), which then calls  deleteEquipmentType() in src/Core/DAO.php (line 632 in CoreService.php).

**Input:**

Bulk delete is not supported. Either '_id' or 'name' must be present in the request JSON.

Sample request JSON:

    {
        "_id" : "58e81cc37f8b9ac4571ef7c6",
        "name" : "laptop"
    }

**Output:**

JSON document with following keys: “ok” - boolean, “msg” - string (message)
