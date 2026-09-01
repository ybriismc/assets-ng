# NetherGames Network - World & Map Assets

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://cdn.nethergames.org/img/logo/one-line-non-flush-light.png">
  <source media="(prefers-color-scheme: light)" srcset="https://cdn.nethergames.org/img/logo/one-line-non-flush-dark.png">
  <img alt="NetherGames" src="https://cdn.nethergames.org/img/logo/one-line-non-flush-dark.png" width="450">
</picture>

<br><br>

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](LICENSE)
[![Plugins Repo](https://img.shields.io/badge/Plugins%20Repo-NetherGamesMC%2Fplugins-blue.svg)](https://github.com/NetherGamesMC/plugins)
[![Minecraft Bedrock](https://img.shields.io/badge/Minecraft%20Bedrock-v1.20.0--v1.26.30-brightgreen.svg)](https://minecraft.net)

**Official world maps, game arenas, waiting lobbies, and configuration data for the NetherGames Network.**

[Closure Announcement](https://support.nethergames.org/closure-announcement) • [Closure FAQ & Info](https://support.nethergames.org/closure-info) • [Plugins Repository](https://github.com/NetherGamesMC/plugins) • [License](LICENSE)

</div>

---

## About

This repository contains the complete collection of world files, custom maps, waiting lobbies, and arena coordinate configurations (`arenas.yml`) used across all minigames and lobbies on NetherGames.

Following the closure of the server on **June 28th, 2026**, these assets have been released under the **Creative Commons Attribution 4.0 International (CC BY 4.0)** license. Anyone is free to use, modify, and host these worlds for their own servers, matches, and events.

---

## Contents

| Directory                                                                                             | Ships                                                                                                         |
|:------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------|
| `Lobby`                                                                                               | `worlds/` — the hub and arcade lobby worlds                                                                   |
| `SkyBlock`                                                                                            | `worlds/` — hub, PvP and arena worlds; `DefaultIslands/` — the seven starter island templates                 |
| `Factions`                                                                                            | `worlds/` — hub, PvP, KOTH and the `wild` overworld; `archive/` — the three original Farlands starting worlds |
| `Bedwars`, `Skywars`, `Duels`, `TheBridge`, `Conquests`, `MurderMystery`, `SurvivalGames`, `Meltdown` | `arenas/`, `arenas.yml`, `WaitingLobby/`                                                                      |
| `MommaSays`, `Soccer`                                                                                 | `arenas/`, `WaitingLobby/`                                                                                    |
| `UHC`                                                                                                 | `WaitingLobby/` — match worlds are generated at runtime                                                       |

---

## Usage with NetherGames Plugins

These map assets are designed to work directly with the open-source plugins in the **[NetherGamesMC/plugins](https://github.com/NetherGamesMC/plugins)** repository.

When configuring server directories or running Docker containers, asset paths map to the following locations:

| Asset                                | Destination                                                              |
|:-------------------------------------|:-------------------------------------------------------------------------|
| `<Game>/worlds/*`                    | `/home/worlds/*` — for gamemodes shipping whole worlds (Lobby, SkyBlock) |
| `<Game>/arenas`, `<Game>/arenas.yml` | `/home/plugin_data/<PluginName>/`                                        |
| `<Game>/WaitingLobby`                | `/home/plugin_data/<PluginName>/WaitingLobby` **and** `/home/worlds/Hub` |
| `SkyBlock/DefaultIslands`            | `/home/plugin_data/NGSkyBlock/DefaultIslands`                            |

`<PluginName>` is the plugin's own name, which is not always `x<Game>` — SkyBlock's plugin is `NGSkyBlock` and the hub's is plain `Lobby`.

> [!IMPORTANT]
> `WaitingLobby` belongs in **both** places. `libminigames` copies it out of the plugin's data folder into a fresh world for every arena it creates, so a server that only has it as `/home/worlds/Hub` will start matches with no waiting lobby.

The plugins repository ships a Docker setup that performs this mapping for you — point `ASSETS_PATH` at a checkout of this repository and the correct files are installed on first boot.

### SkyBlock

SkyBlock is not arena-based. `worlds/` holds the three persistent server worlds — `Hub`, `pvp` and `arena` — while `DefaultIslands/` holds the templates copied when a player creates an island: `Desert`, `Greek`, `Jungle`, `Modern` and `Scrubland` are the types currently offered, with `Snowy` and `Town` retained as extras. Player islands themselves are not in this repository; they live in S3-compatible object storage at runtime.

### Factions

Factions is not arena-based either. `worlds/` holds `Hub`, `FactionsPvP` and `koth`, plus `wild` — the shared overworld that faction land is claimed in.

`wild` is generated by the [VanillaGenerator](https://github.com/NetherGamesMC/VanillaGenerator) plugin when it is absent, so a server will build its own if this directory is removed. The copy here is the world as it stood at closure, with everything players built in it, and it is by far the largest asset in this repository at 36 MB.

#### The archived Farlands worlds

`archive/` holds the three worlds Farlands originally started from, as zip archives of roughly 30 MB each. Each is a small world rather than a finished map: a hand-built spawn and the terrain immediately around it, with everything beyond left for the generator to fill in as players walk into it. NetherGames ran one per region — US, EU and AP — so the three share a spawn build and then diverge indefinitely, each region generating its own terrain outward from the same starting point.

| Archive        | World seed  | Spawn point     | Generator           |
|:---------------|:------------|:----------------|:--------------------|
| `wild-fl1.zip` | `774411`    | `288, 158, 277` | `vanilla_overworld` |
| `wild-fl2.zip` | `31423615`  | `170, 158, 316` | `vanilla_overworld` |
| `wild-fl3.zip` | `960019460` | `201, 158, 326` | `vanilla_overworld` |

Those spawn points matter beyond marking where players appear. Factions measures its safe zone, war zone and spawn leaderboards from whatever spawn point the world carries, so an archived world arrives with its zone geometry already lining up with the build — which is the practical reason to start from one rather than generating fresh terrain and placing a spawn afterwards.

Each archive also holds a `block-data` directory beside the usual `db` and `level.dat`. That one is not a vanilla world file: it is the LevelDB the Factions plugin keeps inside the world folder to record how far raiders have chewed through each block.

To use one, extract it and rename the folder to `wild`, because that is the name the plugin loads — the archives unpack as `wild-fl1`, `wild-fl2` and `wild-fl3`. The name stored inside the world need not match the folder and in one case does not, since PocketMine identifies a world by its directory name.

---

## License

All map and world assets in this repository are licensed under the **[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE)** license.

You are free to:
- **Share**: Copy and redistribute the material in any medium or format.
- **Adapt**: Remix, transform, and build upon the material for any purpose, even commercially.

Under the condition that you give appropriate credit to **NetherGames** and provide a link to the license.
