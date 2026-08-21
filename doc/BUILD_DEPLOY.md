# Pinnacle-Dashboard Build and Deployment

## Purpose and scope

Pinnacle-Dashboard is the web UI in `webui/`. The root Makefile delegates local development, production compilation, Vitest tests, ESLint, and Docker image lifecycle to Next.js/npm and `docker/Makefile`. The package manifest declares Next `16.2.6`, React `19.2.4`, TypeScript, Vitest, and ESLint dependencies.

## 1. Prerequisites

- Node.js/npm compatible with the checked-in `webui/package-lock.json` and the Next/React versions in `webui/package.json`.
- Docker and a running daemon for Docker targets.
- Network access for the first `npm install`, unless the npm cache is complete.
- A configured backend/auth environment for meaningful browser acceptance testing; the Makefile does not provision one.

## 2. Setup

Install locked web dependencies through the root target:

```bash
make install
```

The target runs `cd webui && npm install`. Inspect Make targets with `make` or the comments in the root Makefile.

## 3. Build and compile

Development server (Next.js, default Next port unless overridden by Next configuration):

```bash
make dev
```

Production bundle:

```bash
make build
```

The bundle is generated under `webui/.next/`. The root Makefile does not define a production `start` target; use the package script after building when a deployment process supplies the required environment:

```bash
cd webui && npm run start
```

## 4. Test and lint

```bash
make test                 # cd webui && npm test, Vitest run
make lint                 # cd webui && npm run lint, ESLint
cd webui && npm run type-check
```

`type-check` is a package script but is not wrapped by the root Makefile. Tests are unit-level unless the test suite itself configures browser/backend integration.

## 5. Packaging and artifacts

Build the Docker image with the subordinate Docker Makefile:

```bash
make docker-build
```

The root variables are `IMAGE=pinnacle-dashboard`, `TAG=latest`; override them as needed:

```bash
make docker-build IMAGE=pinnacle-dashboard TAG=1.0.0
```

The primary artifacts are `webui/.next/` for a local bundle and the tagged Docker image for container deployment. There is no npm package publication target.

## 6. Deployment, install, and run

Run the production container in detached mode, using the root `PORT` variable (default `3010`):

```bash
make docker-run
make docker-run IMAGE=pinnacle-dashboard TAG=1.0.0 PORT=3010
```

Inspect and manage the container:

```bash
make docker-logs
make docker-shell
make docker-stop
```

`docker-shell` requires a running container. The exact port binding, runtime environment, and backend URL are defined by `docker/Makefile` and the image configuration; verify them before exposing the service. Credentials, TLS termination, and cloud routing are deployment-specific and are not supplied by this repository's root Makefile.

## 7. Cleanup

```bash
make clean                 # removes webui/.next
make docker-clean          # removes the configured Docker image/tag
```

`make clean` does not remove `node_modules`; remove that directory only as a deliberate local dependency reset.

## 8. Hardware, cloud, and manual verification caveats

- No hardware is required for the local build/test path, but the UI may need a live Pinnacle-HQ/API/OIDC environment to exercise real workflows.
- Docker build/run validates packaging and process startup, not auth redirects, API contracts, WebSocket behavior, or production ingress.
- Validate browser access from the intended hostname, environment variables, CORS/OIDC redirects, TLS, and backend connectivity manually.
- Do not treat `latest` or the development container settings as a production release policy; choose immutable tags and supply credentials through the deployment platform.
