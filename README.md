# Platform Radial Fix

> 🇫🇷 Version française : [README.fr.md](README.fr.md)

A small compatibility theme for Discord that makes the **PlatformIndicators** plugin (Equicord) and the **RadialStatus** theme work nicely together, and restores the **default Discord typing indicator** that RadialStatus breaks.

This theme **does not modify** PlatformIndicators or RadialStatus. It only overrides what is needed on top of them, so both can keep being updated normally.

---

## What does it fix?

| Problem | Without the fix | With the fix |
| --- | --- | --- |
| Typing indicator (the "..." animation) | A large gray circle shifted away from the avatar, and the dots drawn on top of the avatar | The default Discord pill with 3 animated dots, at the bottom-right of the avatar |
| Platform icons layout | Misaligned next to the username in some places | Icons aligned horizontally, spaced consistently (member list, profiles, messages) |
| Mobile phone icon | Duplicated (drawn by both PlatformIndicators and RadialStatus) | Only the PlatformIndicators one is kept |

---

## Requirements

You need **all** of the following:

1. **A Discord client mod that provides the plugin below**
   - [Equicord](https://github.com/Equicord/Equicord)
2. **The PlatformIndicators plugin, enabled**
   - [Equicord/src/plugins/platformIndicators](https://github.com/Equicord/Equicord/tree/main/src/plugins/platformIndicators)
   - Its dependencies (`MemberListDecoratorsAPI`, `MessageDecorationsAPI`, `NicknameIconsAPI`) are enabled automatically by the client mod.
3. **The RadialStatus theme, enabled**
   - [DiscordStyles/RadialStatus](https://github.com/DiscordStyles/RadialStatus)
4. **A Chromium-based Discord client** (the official desktop app is fine), because the theme uses `:has()` and `@property`.

> Without RadialStatus, this theme has nothing to fix. Without PlatformIndicators, the "Platform Indicators" part of the theme is simply unused.

---

## Installation

> ⚠️ **Load order matters.** This theme must be loaded **after** RadialStatus, otherwise RadialStatus' own variables win over the ones defined here.

### Option A: online theme (recommended, stays up to date)

1. Open **Settings → Themes → Online Themes**.
2. Make sure the RadialStatus line is **above** this one, then add:

```
https://raw.githubusercontent.com/TheSakyo/discord-platform-radial-fix/main/platform-radial-fix.theme.css
```

3. Enable it, done.

### Option B: local theme

1. Download [`platform-radial-fix.theme.css`](platform-radial-fix.theme.css).
2. Open **Settings → Themes → Open Themes Folder** and drop the file in it.
3. Enable it **after** RadialStatus.

---

## Customization

Everything is configurable through three CSS variables defined in the `:root` block (section 1 of the file). You can edit the file, or override them from your own CSS:

```css
:root {
    --vc-typing-bg: #2b2d31;   /* Background color of the typing pill */
    --vc-typing-width: 69%;    /* Pill width, relative to the avatar */
    --vc-typing-height: 40%;   /* Pill height, relative to the avatar */
}
```

| Variable | Default | Description |
| --- | --- | --- |
| `--vc-typing-bg` | `#2b2d31` | Background color of the typing pill |
| `--vc-typing-width` | `69%` | Width of the pill, relative to the avatar size |
| `--vc-typing-height` | `40%` | Height of the pill, relative to the avatar size |

Tip: keep the height around **58 % of the width** to preserve the pill proportions.

Because sizes are percentages, the pill scales with every avatar size (32, 40, 80, 120px...). Avatars of 16px are ignored on purpose.

---

## How it works

RadialStatus turns the typing background into a 30px gray circle and forces the coordinates (`cx` / `cy`) of the three dots, which displaces them. CSS cannot revert those values, so instead of fighting them this theme:

1. **Hides** RadialStatus' typing rendering (`display: none !important`).
2. **Detects** an avatar in the "typing" state with `:has(...)`.
3. **Redraws** the Discord pill with an `::after` pseudo-element on the avatar, using three radial gradients as dots.
4. **Animates** each dot's opacity through registered custom properties (`@property`), with a 0.2s delay between dots.

The file is split into numbered sections with a table of contents at the top, so it is easy to read and adjust.

---

## Troubleshooting

**The typing pill does not appear**
- Make sure someone is **actually typing** in the channel while you look at the member list.
- Check that the theme is loaded **after** RadialStatus.
- RadialStatus (and this theme) rely on Discord's hashed class name `wrapper_XpIUHy`. Discord changes these hashes from time to time. If the avatar wrapper class changed, RadialStatus itself stops working too. Inspect an avatar with `Ctrl+Shift+I`, find the new `wrapper_xxxxxx` class and replace `XpIUHy` in this file.

**The pill is too big / too small / the wrong color**
- Adjust the three variables described in [Customization](#customization).

**The phone icon is still duplicated**
- Check the load order: this theme must come after RadialStatus so `--rs-phone-visible: none` wins.

---

## Credits

- **PlatformIndicators**: by kemo, TheSun, Nuckyz, Ven and neoarz, part of [Equicord](https://github.com/Equicord/Equicord) / Vencord.
- **RadialStatus**: by [Gibbu](https://github.com/DiscordStyles/RadialStatus).
- **Platform Radial Fix**: by TheSakyo.

This project is not affiliated with Discord, Equicord, Vencord or DiscordStyles. Client modifications are not endorsed by Discord's Terms of Service: use them at your own risk.

## License

[MIT](LICENSE)
