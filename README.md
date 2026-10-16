# @nebutra/provider-factory

Public mirror for [@nebutra/provider-factory](https://www.npmjs.com/package/%40nebutra%2Fprovider-factory) from [Nebutra/Nebutra-Sailor](https://github.com/Nebutra/Nebutra-Sailor/tree/main/packages/platform/provider-factory).

This repository is generated from the Nebutra Sailor monorepo. Package releases are cut from the monorepo and mirrored here for discovery, standalone cloning, and contribution intake.

- Canonical source: `packages/platform/provider-factory` in `Nebutra/Nebutra-Sailor`
- Package registry: npm and GitHub Packages
- Contributions: open issues or PRs here; maintainers port accepted changes back into the monorepo source package

---
Neutral provider-resolution primitive for provider-agnostic Nebutra packages.

This package centralizes the shared selection rule used by cache, queue, search,
notifications, vault, uploads, and similar packages:

```text
explicit provider -> environment provider -> detector chain -> fallback
```

It does not instantiate concrete providers. Domain packages still own their
provider-specific imports and setup.

## Installation

```bash
pnpm add @nebutra/provider-factory
```

## Usage

```ts
import {
  assertProviderAllowed,
  envPresent,
  resolveProviderType,
} from "@nebutra/provider-factory";

type CacheProvider = "memory" | "redis";

const provider = resolveProviderType<CacheProvider>({
  explicit: undefined,
  envVarName: "CACHE_PROVIDER",
  detectors: [{ provider: "redis", when: envPresent("REDIS_URL") }],
  fallback: "memory",
});

assertProviderAllowed(provider, {
  disallowedInProd: ["memory"],
  overrideEnv: "ALLOW_MEMORY_CACHE_IN_PROD",
  hint: "Configure REDIS_URL or set CACHE_PROVIDER=redis",
});
```

## API

| Export | Description |
| --- | --- |
| `resolveProviderType(input)` | Apply explicit, env, detector, fallback precedence |
| `envPresent(name)` | Build a detector predicate from an environment variable |
| `assertProviderAllowed(provider, options)` | Refuse unsafe providers in production unless explicitly overridden |
| `ProviderDetector<T>` | Detector shape |
| `ResolveProviderInput<T>` | Resolver input shape |
| `ProdGuardOptions<T>` | Production guard options |

## License

MIT
