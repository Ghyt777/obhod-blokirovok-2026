# Bypassing internet censorship in Russia in 2026: what works

A practical guide: which services are blocked, which protocols still get through Russia's DPI system (TSPU), which clients to install and how to connect in five minutes — plus ready-made domain lists for split tunneling.

> **Updated:** October 2026. The situation changes every few weeks — if something is out of date, please open an issue.
>
> 🇷🇺 [Русская версия](../../README.md)

**Contents**

- [What is blocked](#what-is-blocked)
- [Which protocols still work](#which-protocols-still-work)
- [Ways around the blocks](#ways-around-the-blocks)
- [Clients for VLESS, Reality and Hysteria2](#clients-for-vless-reality-and-hysteria2)
- [Connecting in 5 minutes](#connecting-in-5-minutes)
- [Split tunneling](#split-tunneling)
- [Telegram](#telegram)
- [FAQ](#faq)

---

## What is blocked

| Service | Unavailable since |
|---|---|
| YouTube | 10 February 2026 (throttled since 2024) |
| WhatsApp | February 2026 (restrictions since November 2025) |
| Discord | 8 October 2024 |
| Viber | 13 December 2024 |
| Signal | 9 August 2024 |
| Instagram | 14 March 2022 |
| Facebook | 4 March 2022 |
| X (Twitter) | 4 March 2022 |
| LinkedIn | November 2016 |
| Telegram | restrictions since 2025–2026 |

## Which protocols still work

TSPU (the traffic-filtering equipment installed at Russian ISPs) recognises VPNs by what the traffic looks like, not by the server address. So what matters is not "which VPN" but **which protocol**.

| Protocol | Status in 2026 | Why |
|---|---|---|
| **VLESS + Reality** | ✅ works best | indistinguishable from ordinary HTTPS to a large website |
| **VLESS + XHTTP** | ✅ works | looks like plain HTTP requests; helps where Reality is cut |
| **AmneziaWG** | ✅ works | WireGuard with obfuscation — same speed, no recognisable signature |
| **Hysteria2** | ✅ often works | runs over UDP (QUIC); helps on unstable mobile internet |
| **Shadowsocks 2022** | ⚠️ depends on the ISP | sometimes cut by traffic-behaviour analysis |
| **WireGuard** (plain) | ❌ widely blocked | the handshake is easy for DPI to spot |
| **OpenVPN** | ❌ widely blocked | same reason |
| **Cloudflare WARP** | ❌ no longer works as a bypass | gives Russian users Russian exit addresses |
| **PPTP, L2TP/IPsec** | ❌ | outdated and recognised immediately |

A separate problem is **whitelists on mobile internet**: in some regions, at times, only approved sites are reachable and no VPN works at all. That is not a client or provider fault.

## Ways around the blocks

1. **zapret / GoodbyeDPI / ByeDPI** — DPI circumvention without a VPN. Free, the rest of your traffic goes directly, but it only helps against throttling and DPI blocks (YouTube, Discord), not IP blocks. Strategies need tuning per ISP and break from time to time. → [bol-van/zapret](https://github.com/bol-van/zapret)
2. **A ready-made VPN service on VLESS or AmneziaWG** — the simplest option: a key from the provider plus a client (below). Russian services cost about 100–300 ₽ a month, and many offer a trial day for 10–30 ₽, so you can test on your own ISP before paying.
3. **Your own server (a VPS abroad).** [AmneziaVPN](https://github.com/amnezia-vpn/amnezia-client) sets up a server in a couple of clicks; for VLESS there are panels such as [3x-ui](https://github.com/MHSanaei/3x-ui). Full control, but the VPS IP can get blocked and you will have to replace it yourself.
4. **Free VPN apps from app stores** — not recommended: they usually rely on blocked protocols, show ads and monetise user data.

## Clients for VLESS, Reality and Hysteria2

Most working services give you a **key** (`vless://...`) or a **subscription link**, not an app. You paste it into a client:

| Client | Platforms | Open source | Download |
|---|---|---|---|
| Happ | iOS, Android, Windows | no | [happ.su](https://www.happ.su/main) |
| Hiddify | Android, iOS, Windows, macOS, Linux | yes | [GitHub](https://github.com/hiddify/hiddify-app/releases) |
| Streisand | iOS, iPadOS, macOS | no | [App Store](https://apps.apple.com/us/app/streisand/id6450534064) |
| V2RayTun | Android, iOS | no | [Google Play](https://play.google.com/store/apps/details?id=com.v2raytun.android) |
| v2rayNG | Android | yes | [GitHub](https://github.com/2dust/v2rayNG/releases) |
| NekoBox | Android | yes | [GitHub](https://github.com/MatsuriDayo/NekoBoxForAndroid/releases) |

- **iPhone / Mac** — Streisand or Happ
- **Android** — Happ, v2rayNG or NekoBox
- **Windows / Linux** — Hiddify or Happ
- need **Hysteria2 or TUIC** — Hiddify, Streisand or NekoBox

> In March 2026 Happ, Streisand and V2RayTun were removed from the Russian App Store. Installed apps keep working; a new install needs an Apple ID from another country.

> Download clients only from official sources: search results are full of look-alike "official sites" that sell their own VPN under someone else's name.

## Connecting in 5 minutes

1. Get a key or subscription link from your VPN service and copy it.
2. Install a client for your platform (table above).
3. In the client tap **"+"** → **"Import from clipboard"** (or scan the QR code).
4. Pick a server, tap connect, and allow the VPN configuration.
5. Turn on **routing for Russia** (in Hiddify set the region to "Russia"; in v2rayNG and Happ use routing rules) so banks and government services open directly.

Not connecting? Try other servers from the subscription, refresh the subscription, check Wi-Fi vs. mobile data (if Wi-Fi works, a mobile whitelist is likely), and try another protocol if your provider offers several (Reality → XHTTP → Hysteria2).

## Split tunneling

Sending all traffic through the VPN breaks Russian banking apps and government services, which reject foreign IPs. Route only blocked services through the VPN — ready-made lists in this repository:

- [`lists/blocked-services.txt`](../../lists/blocked-services.txt) — domains of services blocked in Russia;
- [`lists/ru-direct.txt`](../../lists/ru-direct.txt) — what to send directly: .ru, .su, .рф and Russian services on other domains;
- [`lists/xray-routing-example.json`](../../lists/xray-routing-example.json) — example routing rules for Xray-based clients.

Details (in Russian): [split tunneling](../ru/09-split-tunneling.md).

## Telegram

For Telegram alone you don't need a VPN — an **MTProto proxy** is built into the app (Settings → Advanced → Connection type → Use custom proxy). If the "Connect proxy" button in a browser does nothing, Telegram isn't registered as the `tg://` link handler (common with the portable and Microsoft Store versions) — add the proxy manually: server, port and secret.

## FAQ

**Is using a VPN legal in Russia?**
At the time of writing, using a VPN as a private individual is not punished in itself. Advertising and "popularising" circumvention tools is banned, and since September 2025 deliberately searching for extremist materials — including via a VPN — is an administrative offence. Laws change; follow the news.

**Why does the VPN work on Wi-Fi but not on mobile internet?**
Mobile operators filter differently, and some regions use whitelists. Try another server or protocol — XHTTP or Hysteria2 often help.

**Is a free VPN OK?**
For a one-off task, maybe. For daily use, no: free app-store VPNs usually run blocked protocols and monetise your data.

---

Maintained by the author of [vpnmarket.online](https://www.vpnmarket.online), a VPN catalog whose provider data (prices, protocols, trials) is checked against the providers' official sites. Some VPN links in the catalog are affiliate links. This guide is informational and does not encourage breaking the law.

Found a mistake? [Open an issue](https://github.com/Seitmaxudov/obhod-blokirovok-2026/issues) or send a pull request.
