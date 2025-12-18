This folder is mounted into the container at /config (read-only).

Use it for:
- operational notes and runbooks
- keeping a known-good HAProxy config if you choose to override the image defaults
- storing certificate artifacts if you later switch to a manual TLS approach

IMPORTANT:
Do not override the container's HAProxy config until you have extracted the exact
internal config path used by your image tag and validated the config syntax.
