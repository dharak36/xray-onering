# Xray-Core

[Xray-core](https://github.com/XTLS/Xray-core) is a tool used to bypass ISP restrictions and is commonly used as a free-internet solution. See the official [README](https://github.com/XTLS/Xray-core#readme) for more details.

# Onering Method

![Remnawave](https://raw.githubusercontent.com/dharak36/xray-onering/refs/heads/main/onering.png)

A new method that supports WebSocket bugs, SNI bugs, and Wildcard bugs without pointing any domain to Cloudflare.

# Origin

The idea of bypassing using a wildcard first appeared for me in early 2022 and was kept for private use until March 2024, when I first released it in a WhatsApp group. I didn’t like the original method because it required Cloudflare’s Advanced SSL Manager, so I developed two new methods.

The first one is still in beta because it requires specific features that Cloudflare has not yet implemented.
The second method is what I call **“Onering”**, because it supports any bug that requires WebSocket, SNI, or Wildcard behavior, and it requires **no domain pointing**.

## Scanning Bugs

This method is designed with Cloudflare CDN in mind (CloudFront is also supported), so you still need to set up Cloudflare CDN. Below is the syntax for scanning bugs. Just open a shell or Termux.

### Downloading the Bug Scanner on Termux (Android)

```bash
wget -O bugscanner https://github.com/dharak36/xray-onering/blob/main/bugscanner.linux.arm64.64bit
```

Then create a file anywhere you like—here we’ll use `/sdcard/host.txt`. Add the Cloudflare bugs you want to test, one per line. Example:

```bash
support.zoom.us
ava.games.naver.com
ads.ruangguru.com
```

### Running the Scanner

Use the following parameters:

* **host**: location of the host file (e.g., `/sdcard/host.txt`)
* **result**: location of the result (e.g., `/sdcard/result.txt`)
* **server**: your V2Ray/Xray server domain running behind Cloudflare CDN (e.g., `www.yourdomain.com`)
* **cloudflare**: `true/false` (default is `false` if empty. If `true`, it will only check hosts using Cloudflare and ignore non-Cloudflare results.)

Example:

```bash
./bugscanner -host /sdcard/host.txt -result /sdcard/result.txt -server www.yourdomain.com -cloudflare true
```

## Xray-Core + Onering Usage

Usage is straightforward. Here is an example using `support.zoom.us` as the bug domain.
The `sni/servername` format is:

```
onering:yourdomain:bugdomain
```

Example configuration:

```bash
server: support.zoom.us
port: 443
host: www.yourdomain.com
sni/servername: onering:www.yourdomain.com:support.zoom.us
type: websocket
```

Example configuration V2rayNG:

```bash
vmess://eyJhZGQiOiJzdXBwb3J0Lnpvb20udXMiLCJhaWQiOiIwIiwiYWxwbiI6IiIsImZwIjoiIiwiaG9zdCI6IjEwLmxvd2gubmV0IiwiaWQiOiIwMDAwMDAwMC0wMDAwLTAwMDAtMDAwMC0wMDAwMDAwMDAwMDAiLCJuZXQiOiJ3cyIsInBhdGgiOiIvaGVsbG8iLCJwb3J0IjoiNDQzIiwicHMiOiIxMC5sb3doLm5ldCIsInNjeSI6ImF1dG8iLCJzbmkiOiJvbmVyaW5nOjEwLmxvd2gubmV0OnN1cHBvcnQuem9vbS51cyIsInRscyI6InRscyIsInR5cGUiOiItLS0iLCJ2IjoiMiJ9
```
