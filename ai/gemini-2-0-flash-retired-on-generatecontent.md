# `gemini-2.0-flash` 404s on `generateContent`

Calls to the Gemini API started failing with:

```
404 ... model "gemini-2.0-flash" is no longer available ...
```

The maddening part: `ListModels` **still lists** `gemini-2.0-flash`, so the model id *looks* valid and you waste time blaming auth, the endpoint, or the request body. The listing endpoint and the `generateContent` availability had drifted apart.

## Fix

Swap to a current model id — `gemini-2.5-flash` was a drop-in for me, same request shape including `responseSchema` structured-JSON calls:

```diff
- "models/gemini-2.0-flash:generateContent"
+ "models/gemini-2.5-flash:generateContent"
```

Then grep the codebase for other stale ids (`gemini-1.5-*`, `gemini-2.0-*`) before they bite in another service.

## Takeaway

For hosted LLM APIs, don't trust the model-listing endpoint to prove a model is callable — providers retire model ids on `generateContent` while still listing them. Pin current ids in config and grep for stragglers when one 404s.
