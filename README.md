# Counter-Strike 1.6 Egg

This repository contains a Pelican egg for running a Linux Counter-Strike 1.6
dedicated server. Its description targets ReHLDS and it can install ReHLDS,
MetaMod-R, AMX Mod X, ReGameDLL_CS, ReAPI, and ReUnion.

## Repository contents

- [`egg-counter-strike16.json`](./egg-counter-strike16.json) — the Pelican egg
  export, including its install script, startup command, settings, and embedded
  icon.

The JSON is a panel configuration, not a copy of the game server. SteamCMD and
the installer download the game files and selected components when a server is
installed.

## What the egg installs

The installer uses SteamCMD to install the Half-Life dedicated server
(Steam application ID `90`) and the selected game directory (by default,
`cstrike`). It then downloads Linux binaries and packages from their upstream
release repositories as requested by **Available modules**:

- **ReHLDS** — optimized, compatible replacement for the original HLDS server
  binaries.
- **AMX Mod X** — server-side scripting and administration framework.
- **MetaMod-R** — plugin loader used by compatible server plugins.
- **ReGameDLL_CS** — Counter-Strike game DLL implementation.
- **ReAPI** — ReHLDS/ReGameDLL scripting API.
- **ReUnion** — Steam/non-Steam player authentication support.

The egg's default **Available modules** value is
`rehlds,reunion,amxmodx,metamod-r,reapi,ReGameDLL_CS`, so all six components
above are installed by default. Module names are matched without regard to
case.

The installer fetches a `server.cfg` from the URL configured in its install
script (by default, a raw GitHub URL). The panel's egg configuration then
manages the FastDL and download/upload settings in `cstrike/server.cfg`.

## Egg settings

| Setting | Environment variable | Default | Purpose |
| --- | --- | --- | --- |
| Game ID | `HLDS_APPID` | `90` | Steam application ID for HLDS. The egg description says not to change it. |
| Beta branch | `HLDS_BRANCH` | `steam_legacy` | SteamCMD branch to install. Leave empty to use the public branch. |
| Game Name | `HLDS_GAME` | `cstrike` | Game/mod directory passed to HLDS. |
| Available modules | `INSTALL_MODULES` | `rehlds,reunion,amxmodx,metamod-r,reapi,ReGameDLL_CS` | Comma-separated components to install. |
| Map | `MAP` | `de_dust2` | Map started by default. |
| Max Players | `MAXPLAYERS` | `14` | Maximum player count passed to HLDS. |
| ReHLDS Version | `REHLDS_VERSION` | `latest` | Declared version setting. The current install script selects the latest matching ReHLDS release asset, so changing this setting does not currently pin a version. |
| sys_ticrate | `SYS_TICRATE` | `1024` | Maximum server tick rate passed to HLDS. |
| Timezone | `TZ_DATE` | `Europe/Bucharest` | Container timezone. |

The startup command binds to `0.0.0.0` and uses the server's allocated port. It
also sets the Steam query port to `26900` and starts the configured map and
player limit.

## Use

1. Import `egg-counter-strike16.json` into Pelican as an egg.
2. Create a server using this egg and assign its game port.
3. Set the egg variables as needed, especially the branch, map, player limit,
   and optional modules.
4. Install the server. The installer needs outbound internet access to Steam
   and the upstream release/configuration sources.
5. Start the server from the panel.

Installed files are managed by the panel's server directory; they are not
checked into this repository. Review the installer script and its upstream
download sources before deploying, particularly if you change the default
server configuration URL.
