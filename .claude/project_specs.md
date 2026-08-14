# Pinnacle-Dashboard — specs Remote: `git@github.com:soccentric-pinnacle/Pinnacle-Dashboard.git`
Stack: node. A standalone dashboard UI leaf. Runs anywhere; not part of the on-device image. ## Layout * `webui/` — Next.js app-router frontend. Has its own `package.json`, separate from the root one.
* `contract-tests/` — pytest suite with its own Makefile. Python, not JS.
* `docker/` — packaging. ## Coupling * **Not proto-coupled to Pinnacle-HAL.** Reads from Pinnacle-Server over HTTP/SSE.
* That HTTP/SSE surface is the whole dependency, and it is not compile-checked — which is why `contract-tests/` exists, and why a Server-side change can break this repo without touching its code.
* **Distinct from Pinnacle-Configuration's on-device dashboard.** Configuration's UI ships onto the NXP i.MX 93 inside the OS image. This one does not.
