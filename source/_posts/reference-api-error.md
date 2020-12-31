---
title: API Error response
---

## Error response
### HTTP Status
|Status code|Reason|
|---|---|
|400|Request parameter is invalid.|
|401|Error authentication.|
|403|Permission denied.|
|404|Notfound data or Notfound resource.|
|405|HTTP Method not allowed.|
|405|HTTP Method not allowed.|
|409|Conflict data or Conflict resource.|
|500|Server error.|

### Response body
|Value|Schema|Description|
|---|---|---|
|code|Boolean|Status code.|
|title|Boolean|Error title.|
|message|Boolean|Error message.|
|error_code|String|Error identifier code. If you implement handling, use this.|
