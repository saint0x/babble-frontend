# Babble frontend

The Astro frontend for [Babble Protocol](https://github.com/saint0x/babble-protocol), with the matching TypeScript SDK and protocol fixtures included so a fresh checkout can build and run its client tests independently.

This repository preserves the protocol workspace's relative paths:

- `frontend/`: the Astro application, styles, and frontend tests.
- `sdk/`: the TypeScript SDK, generated protocol types, generator, and SDK tests.
- `fixtures/protocol/v1/`: the shared schema bundle and contract fixtures used by both test suites.

The application and SDK source are maintained together in the protocol repository. This repository publishes that source without rewriting imports or requiring a sibling checkout. The root package provides convenience commands; the application's locked dependencies remain in `frontend/package-lock.json`.

## Install and develop

Use Node.js 22.12 or newer and npm 9.6.5 or newer.

```sh
npm ci
npm run build
npm run dev
```

The root install step runs `npm ci` in `frontend/`. Building compiles the included SDK before Astro, so development can resolve the SDK from a fresh checkout. The development server binds to `127.0.0.1` and uses port 4321 by default.

The UI connects to `http://127.0.0.1:8787` by default. Run the Babble API from the protocol repository, or provide another API URL when starting development:

```sh
PUBLIC_BABBLE_API_URL=https://your-babble-node.example npm run dev
```

The backend must permit requests from the frontend origin. This repository does not start or deploy the backend.

## Validate and build

```sh
npm run check
npm test
PUBLIC_BABBLE_API_URL=https://your-babble-node.example npm run build
npm run preview
```

`check` verifies that generated SDK types match the included schema bundle, builds the SDK, and runs Astro's type checks. `test` performs that check and runs both the SDK and frontend test suites. `build` verifies the generated types, builds the SDK, and creates the static site in `frontend/dist/`. `preview` serves that production build locally.

`PUBLIC_BABBLE_API_URL` is embedded in the generated HTML at build time. Set it for the intended backend before producing deployment output. Changing it only when starting `preview` will not change a previously built site.

To pass a server option through the root scripts:

```sh
npm run dev -- --port 4322
npm run preview -- --port 4322
```

Full system acceptance—including the Rust backend, isolated stores, Aegis browser flows, and Fozzy trace verification—lives in the [protocol repository's test harness](https://github.com/saint0x/babble-protocol/tree/main/tests). Those integrated runs require the complete protocol checkout. A successful frontend build or unit test run does not establish full system production readiness; see the [production-readiness ledger](https://github.com/saint0x/babble-protocol/blob/main/docs/production-readiness.md) for current evidence and remaining work.
