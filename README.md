{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Brik",
  "type": "object",
  "required": ["id","version","name","description","kind","inputs","outputs","instructions","created_by","created_at"],
  "properties": {
    "id": {"type":"string","pattern":"^[a-z0-9._-]+$"},
    "version": {"type":"string","pattern":"^\\d+\\.\\d+\\.\\d+$"},
    "name": {"type":"string"},
    "description": {"type":"string"},
    "kind": {"type":"string","enum":["ui","service","data","workflow"]},
    "inputs": {
      "type":"array",
      "items": {
        "type":"object",
        "required":["name","type"],
        "properties": {
          "name":{"type":"string"},
          "type":{"type":"string"},
          "required":{"type":"boolean"},
          "schema":{"type":"object"},
          "uiHint":{"type":"object"}
        }
      }
    },
    "outputs": {
      "type":"array",
      "items": {
        "type":"object",
        "required":["name","type"],
        "properties": {
          "name":{"type":"string"},
          "type":{"type":"string"},
          "schema":{"type":"object"}
        }
      }
    },
    "ui":{"type":["object","null"]},
    "logic":{"type":["object","null"]},
    "instructions":{"type":"string"},
    "tests":{"type":"array"},
    "compatibility":{"type":"object"},
    "created_by":{"type":"string"},
    "created_at":{"type":"string","format":"date-time"},
    "tags":{"type":"array","items":{"type":"string"}}
  },
  "additionalProperties": true
}
