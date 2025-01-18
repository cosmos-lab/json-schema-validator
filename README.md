# JSON Schema Validator

## **Aim**

This project aims to design a JSON schema that can validate another JSON schema. Here is the initial draft I have created to validate the basic schema provided in the example.

## **Validator JSON Schema**
```
  {
    "type": "object",
    "properties": {
      "type": {
        "enum": [
          "string",
          "number",
          "integer",
          "boolean",
          "object",
          "array",
          "null"
        ]
      },
      "properties": {
        "type": "object",
        "additionalProperties": {
          "$ref": "#"
        }
      },
      "items": {
        "type": "object",
        "$ref": "#"
      },
      "required": {
        "type": "array"
      }
    },
    "required": [
      "type"
    ],
    "if": {
      "properties": {
        "type": { "const": "array" }
      }
    },
    "then": {
      "properties": {
        "items": { "type": "object" }
      },
      "required": ["items"]
    },
    "else": {
      "if": {
        "properties": {
          "type": { "const": "object" }
        }
      },
      "then": {
        "properties": {
          "properties": { "type": "object" }
        },
        "required": ["properties"]
      }
    }
  }
  
```
## **Example Schema**
```
{
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "default": "Test"
    },
    "details": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "id": {
            "type": "integer"
          },
          "description": {
            "type": "string"
          }
        },
        "required": ["id"]
      }
    }
  },
  "required": [
    "name"
  ]
}

```
