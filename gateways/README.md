# Gateways

---
Find the latest information at https://unifiedpush.org/developers/gateway/
---

See the [definition](https://github.com/UnifiedPush/specifications/blob/main/definitions.md#push-gateway).

Gateways are meant to be hosted by the application developpers. To improve sel-hosting, gateways and applications may implement a discover functionnality.

We recommend doing so by returning the following json to a get : `{"unifiedpush":{"gateway":"application"}}` 
