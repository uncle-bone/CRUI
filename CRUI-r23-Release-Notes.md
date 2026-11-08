# CRUI r23 — Stable Release

This is the stable CRUI package based on the current r23 build.

## Changes
- Renamed the NaowhUI companion folder to `EllesmereUINaowhUI`, updating its metadata, texture paths, and registration identity.
- Migrates existing NaowhUI settings stored under the old EllesmereUI profile key.
- Carries forward the r22 class-resource and DataBars default changes and integrated NaowhUI companion settings.
- Includes the NaowhUI companion license; ElvUI and WeakAuras payloads are not included.

## Installation
Extract `CRUI-r23.zip` into the Conquest Reborn `Interface/AddOns` folder. If upgrading from r22, remove the old `NaowhUI_EUI` folder first so only the renamed companion loads.

## Verification note
The r23 folder rename and saved-setting migration have not been verified in-game. The report that additional Top Bar settings were not visible in r22 remains open for follow-up.
