# Contrib

---
Find the latest information at <https://unifiedpush.org>
---

Gateways, Proxies, and more

You can find definitions of Gateway and Rewrite proxy on the [specifications](https://github.com/UnifiedPush/specifications).

TL;DR:

* Gateway and rewrite proxies are used to translate application server protocol to push provider server protocol.
* Gateway is application specific: it translate the application protocol to a raw POST on a specific URL, without specific header.
* Rewrite proxy is provider specific: it translate the raw POST on a specific URL to the push provider server protocol.
