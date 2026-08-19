# Feature Tracker

This fork is developed as a set of isolated features that can be integrated locally while still remaining suitable for separate upstream pull requests.

## Branch model

- `main` — integrated LanKing version. Every accepted local feature eventually lands here.
- `feat/Fxxx-name` — feature development branch based on the current `main`.
- `pr/Fxxx-name` — optional clean upstream-PR branch rebuilt from the current `BSkando/GoogleFindMy-Card:main` and containing only that feature.

Do not open upstream PRs directly from `main` or `feat/*`.

## Features

| ID | Feature | Local status | In `main` | Upstream PR |
|---|---|---|---|---|
| F001 | Reliable device actions | Testing | No | Not prepared |

## Status rules

- **Planned** — scope recorded, implementation not started.
- **Development** — implementation in progress.
- **Testing** — implementation exists locally and is being validated.
- **Ready** — feature has passed the agreed checks and can be merged to `main`.
- **Integrated** — feature is present in `main`.

Each feature has a detailed record under `docs/development/features/`.
