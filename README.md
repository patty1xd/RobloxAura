# 🟣 AURA DEBT SIMULATOR

> Start at **−1000 Aura**. Grind absurd brainrot activities, hatch cursed pets, escape your debt, rebirth, and compete globally. A production-structured Roblox simulator built for retention, virality, and ethical monetization.

This repository is a **Rojo project** containing the full game design, economy, security architecture, and production-quality Luau implementation.

---

## 📦 What's here

| Area | Location | Highlights |
|---|---|---|
| **Design docs** | `docs/` | GDD, Economy (with formulas), Security, Monetization, Architecture, Roadmap |
| **Shared** | `src/shared/` | BigNumber currency, typed remotes + guard, rate limiter, config data |
| **Config (data)** | `src/shared/Config/` | **54 activities**, **106 pets** + 8 eggs, **100 live events**, rebirths/zones, upgrades, gamepasses, products, quests/BP, balance |
| **Server** | `src/server/Services/` | 16 services: Data (session-locked), Economy (sole currency authority), Activity, Pet, Trade (escrow), Rebirth, Quest, Event, Leaderboard, Monetization, AntiExploit, Analytics… |
| **Client** | `src/client/` | HUD, Activities, Shop, Inventory, Pets/Fusion, Trade, Settings, Effects (juice) |

~6,300 lines of Luau across 54 files.

## 🚀 Build & run

1. Install [Rojo](https://rojo.space) (`rojo` CLI or the Studio plugin) and [Aftman](https://github.com/LPGhatguy/aftman) (optional).
2. From this folder: `rojo serve` and connect from Roblox Studio (Rojo plugin → Connect), **or** `rojo build -o AuraDebtSimulator.rbxlx` and open the file.
3. Press **Play**. The server bootstraps all services; the client builds the HUD.

### Before publishing (required)
- Replace every `assetId = 0` in `Config/Gamepasses.lua` and `Config/Products.lua` with your real Roblox asset ids.
- Enable **Studio Access to API Services** (DataStores) and **HTTP Requests** (only if you set an analytics `WEBHOOK_URL`).
- Set the `WEBHOOK_URL` in `AnalyticsService.lua` if you want external telemetry (optional; Roblox AnalyticsService works without it).

## 🔒 Security model (one line)

The client sends **intents only**; the **server owns all state and math**, validates + rate-limits every remote, rolls all RNG, escrows trades atomically (uids are moved, never minted), session-locks DataStore writes, and runs behavioral anti-cheat with strike decay. See `docs/SECURITY.md`.

## 📈 Economy (one line)

Scientific-notation `BigNumber` currency; reward = `base × difficulty × rebirthMult × pets × upgrades × gamepass × event × combo`, with costs growing faster than gains per rebirth to stretch progression to ~6 months with **no hyperinflation**. All constants live in `Config/Balance.lua`. See `docs/ECONOMY.md`.

## 🧩 Extending (the content treadmill)

Most updates are **data-only PRs** to `Config/*` — add an activity, a pet, an egg, or an event with no logic changes. That's the design intent behind the config-as-data architecture (`docs/ARCHITECTURE.md`, `docs/ROADMAP.md`).

## ⚠️ Scope notes

This is a complete, coherent, runnable **foundation** with every core system implemented for real. To ship a polished hit you'd still add: authored 3D zones/maps, pet meshes & VFX, music/SFX, a moderation admin panel UI (the server hooks exist), the battle-pass milestone-pet grant wiring, and a Studio art pass on the programmatic UI. The code is structured so all of that is additive.
