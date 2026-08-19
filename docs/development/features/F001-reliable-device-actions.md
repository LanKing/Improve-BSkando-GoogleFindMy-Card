# F001 — Reliable device actions

## Goal

Make Google Find My device actions work reliably from the card without depending on the integration's fragile custom-service device-ID resolution.

## Original problem

`Play sound` from the card failed with an error like:

```text
Device casma could not be found or is not part of this integration.
```

The same device could play sound successfully from its native Home Assistant device page.

## Root cause discovered

The card stripped the Home Assistant entity domain:

```js
const deviceId = entityId.split('.')[1];
```

For `device_tracker.casma` this produced only `casma`, which is not a Home Assistant Device Registry ID and is not guaranteed to be the canonical Google device ID.

Further inspection also found that the current GoogleFindMy-HA service resolver can map a valid Home Assistant entity/device to a composite integration identifier after the multi-account refactor, while the coordinator expects the raw canonical Google device ID. Therefore simply passing the full tracker entity ID to the custom service is not sufficient.

## Implementation approach

Use Home Assistant's Entity Registry to map actions through the entities created by the integration itself:

1. Start with the selected `device_tracker.*` entity.
2. Read `config/entity_registry/list` over the Home Assistant WebSocket API.
3. Find the tracker's Home Assistant `device_id`.
4. Find the matching `googlefindmy` `button.*` entity on the same device for the requested action.
5. Call Home Assistant's standard `button.press` service.

This mirrors the already-working action path on the native Home Assistant device page and avoids duplicating Google device-ID resolution in the card.

## Scope

- Remove action use of `entityId.split('.')[1]`.
- Resolve integration action buttons through the Entity Registry.
- `Play sound` uses the integration's native Play Sound button entity.
- `Stop sound` uses the integration's native Stop Sound button entity.
- Present Play/Stop as one toggle button in the card.
- `Locate device` should use the same resolver when exposed by the card.
- Fix action-button CSS so icon size and text layout remain correct.

## Play/Stop state

Home Assistant `button` entities are stateless. The card therefore keeps a local set of devices for which it successfully initiated Play Sound:

- successful Play → show Stop;
- successful Stop → show Play;
- page/card reload → state resets to Play.

Limitation: if sound is started outside this card, the card cannot currently know that the tracker is ringing because the integration does not expose a ringing state entity.

## Test status

- [x] Play Sound works from the local modified card.
- [ ] Stop Sound tested with the single toggle button.
- [ ] Toggle returns to Play after successful Stop.
- [ ] Locate Device tested through Entity Registry resolution.
- [ ] Action-button layout checked on desktop.
- [ ] Action-button layout checked on mobile/narrow card.
- [ ] Feature code committed to `feat/F001-reliable-device-actions`.
- [ ] Merged into `main`.

## Upstream status

No upstream PR has been prepared or opened.

If an upstream PR is later approved, create a clean `pr/F001-reliable-device-actions` branch from the then-current `BSkando/GoogleFindMy-Card:main` and port only F001 changes.
