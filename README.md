# 4MG Moderation Bot

**Open-source Discord moderation bot by [4 Million Games](https://github.com/)**

A full moderation toolkit for Discord: bans, mutes, warns, kicks, appeals, roles, command permissions, and a panel-based AutoMod system. Data can be stored in a local JSON file, MongoDB, or a private Discord channel.

---

## Features

### Moderation
- **Ban** — add, remove, edit, list
- **Tempban** — timed bans with auto-expiry
- **Softban** — ban + purge messages, then unban
- **Kick** — kick with case logging
- **Mute** — Discord timeouts with duration
- **Warn** — warnings with case IDs
- **Cases** — look up any punishment by case ID
- DMs sent to users when they are punished

### AutoMod (`/automod`)
Interactive **control panel** (buttons & menus), not a wall of subcommands.

| Rule | Description |
|------|-------------|
| Anti Link | Blocks **all** links except domains you allowlist |
| Anti Invite | Discord invite links |
| Caps Spam | Excessive uppercase |
| Spam | Rapid messages in one channel |
| Channel Spam | Posting across many channels quickly |
| Mass Mention | Too many @users |
| Emoji Spam | Too many emojis |
| Bad Words | Custom word filter |
| Zalgo | Cursed / combining characters |
| Duplicates | Repeated identical messages |
| Newlines | Excessive line breaks |
| Everyone/Here | `@everyone` / `@here` without permission |
| Attachments | Too many files or blocked extensions |
| Stickers | Too many stickers |

**Actions:** delete · warn · mute · kick · ban · softban · tempban  
(Uses the same case system as manual moderation. Duration applies to mute & tempban only.)

**Bypasses:** global channel/role ignore, plus per-rule bypasses.

### Appeals
Users can appeal punishments with `/appeal` — **including via DM** if they are banned. Staff review with buttons or commands. Accepted ban/mute appeals can reverse the punishment automatically.

### Roles
`/role add` · `remove` · `temp` · `info`

### Permissions
`/cmdpermit` — grant command access to specific users or roles (supports subcommands).

### Help
`/help` — command overview · `/help command:` — feature details

---

## Requirements

- **Node.js** 18+
- A Discord application + bot ([Developer Portal](https://discord.com/developers/applications))
- Privileged intents enabled in the portal:
  - **Server Members Intent**
  - **Message Content Intent** (required for AutoMod)

---

## Setup

### 1. Clone / download

```bash
git clone <your-repo-url>
cd discord-moderation-bot
npm install
```

### 2. Environment

Copy `.env.example` to `.env` and fill in:

```env
DISCORD_TOKEN=your_bot_token
CLIENT_ID=your_application_id
GUILD_ID=               # optional — set for instant slash-command updates while testing
OWNER_IDS=your_user_id  # comma-separated; always full access

# Storage — pick ONE
STORAGE_TYPE=discord    # discord | file | mongodb
DATA_CHANNEL_ID=        # required if STORAGE_TYPE=discord
MONGODB_URI=            # required if STORAGE_TYPE=mongodb
```

### 3. Storage options

| `STORAGE_TYPE` | What it does |
|----------------|--------------|
| **`discord`** | Uploads `modbot-data.json` to a private channel. **Recommended** for hosting. Requires `DATA_CHANNEL_ID`. Bot needs View Channel, Send Messages, Attach Files, Read Message History. |
| **`file`** | Local `./data/modbot-data.json` — simplest for development. |
| **`mongodb`** | MongoDB database via `MONGODB_URI`. |

Cases, appeals, permissions, **and AutoMod configs** all save through this same storage layer.

### 4. Invite the bot

OAuth2 URL generator → scopes: `bot`, `applications.commands`  
Permissions (minimum): Manage Roles, Kick, Ban, Moderate Members, Manage Messages, Send Messages, Embed Links, Attach Files, Read Message History.

### 5. Run

```bash
npm start
```

Slash commands refresh automatically on startup. With `GUILD_ID` set, updates are instant for that server.

---

## Usage examples

```
/automod                          → open AutoMod panel
/ban add user:@User reason:raid
/mute add user:@User reason:spam duration:10m
/warn add user:@User reason:language
/appeal submit case_id:12 reason:I was wrongly muted
/role temp user:@User role:@VIP duration:1d
/cmdpermit add command:ban.add role:@Moderator
/help
```

**Banned users:** DM the bot → `/appeal mycases server_id:ID` → `/appeal submit ...`

---

## Project structure

```
├── index.js                 # Bot entry, intents, command refresh, AutoMod hook
├── package.json
├── .env.example
├── LICENSE                  # MIT — Copyright 4 Million Games
├── README.md
└── src/
    ├── config.js
    ├── commands/            # Slash commands (ban, mute, automod, appeal, …)
    ├── storage/             # file | mongodb | discord channel backends
    └── utils/               # duration, DM, permissions, expiry
```

---

## License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE).

```
Copyright (c) 2026 4 Million Games
```

You may use, modify, and distribute this software freely, including commercially, as long as the copyright and license notice are preserved.

---

## Credits

**4 Million Games**

Built with [discord.js](https://discord.js.org/).

---

## Disclaimer

This bot is not affiliated with Discord Inc. Use in accordance with the [Discord Terms of Service](https://discord.com/terms) and [Developer Policy](https://discord.com/developers/docs/policy). You are responsible for how you configure and operate the bot on your servers.
