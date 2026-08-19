# Development Workflow

## Objective

Keep the fork's `main` branch as the complete integrated version while preserving the ability to propose each feature to upstream independently.

## Local feature development

1. Assign the next sequential feature ID: `F001`, `F002`, ...
2. Record the feature scope in `docs/development/features/Fxxx-name.md` before or when implementation starts.
3. Create `feat/Fxxx-name` from the current `main`.
4. Keep commits for that feature prefixed with its ID, for example:
   - `F002: add all-device markers`
   - `F002: fix marker selection`
5. Test the feature locally.
6. When accepted, merge the feature into `main` and update `FEATURES.md` plus the feature record.

A later feature may build on features already integrated into `main`.

## Upstream pull requests

An upstream PR is a separate packaging step, not the development branch.

When a feature is worth proposing to BSkando:

1. Refresh against the latest `BSkando/GoogleFindMy-Card:main`.
2. Create `pr/Fxxx-name` from that upstream state.
3. Port only the commits/changes belonging to that feature.
4. Validate the isolated branch again.
5. Open a PR from `LanKing:pr/Fxxx-name` to `BSkando:main` only after explicit approval.

This prevents unrelated LanKing features from leaking into an upstream PR.

## Feature records

Each feature record should preserve:

- goal;
- original problem or motivation;
- exact scope;
- implementation approach;
- dependencies on other features;
- local test status;
- `main` integration status;
- upstream PR status;
- important implementation notes discovered during debugging.

## Rule

Never open an upstream PR automatically. Preparing a feature locally or merging it into this fork's `main` does not imply permission to publish a PR upstream.
