# How is each metric in a proxy comparison calculated?

**Each metric in a Caiman Check report is calculated from requests sent under the same conditions for both providers, and the formula for every one is public.** This page lists them; the rules for naming a winner are in [How does Caiman Check decide the winner?](how-the-winner-is-decided.md).

## How is the success rate calculated?

The success rate is the share of requests that returned the expected page. The ± next to it is a 95% Wilson confidence interval: with 100 requests and 90% success it is about ±6 points, widened when exits repeat.

## How is the price per 1,000 successful requests calculated?

The price of 1,000 successful requests is: **price per GB × all traffic in GB ÷ successful requests × 1,000**. A provider that is more expensive per GB can be cheaper per result, because failed requests also cost traffic.

Traffic is counted the way a provider bills it: bytes received before decompression, plus headers, request size and an estimated 6 KB TLS handshake per new HTTPS connection. Failed requests count what arrived before the failure. A provider's own meter may differ by 5–15%, equally for A and B.

The price of 1,000 GB of useful traffic is: **price per GB × 1,000 × all traffic ÷ traffic of successful requests**.

## How is response time measured?

Response time is the time from the start of a request to the full answer, for successful requests only, taken from the network library's counter so load on our server doesn't distort it. The report shows the median and the 90th percentile, split into phases:

- connecting to the proxy (entry server),
- the proxy opening a channel to the site (exit device, including TLS),
- the site's response (to the first byte),
- download and redirects.

A slow proxy connection is usually fixed by another entry server; a slow channel depends on the exit devices. Next to speed the report also shows the share of all requests answered correctly within 5 seconds and the timeout share.

## How is UDP checked?

UDP is checked by opening a SOCKS5 UDP channel and sending packets with a unique random tag to Caiman Check's server on ports 3478, 19302 and 8443 (STUN/WebRTC ports). It counts as working only if our server confirms it received that exact tag and replied in its own format; a proxy that just bounces packets back is detected. Every line is tested, up to 3 tries each. HTTP proxies have no UDP.

## How is the system (OS) match checked?

The system match is checked by reading the first TCP packet from the exit device, as antifraud systems do (passive OS fingerprinting, p0f).

| Signal | Windows | Linux / Android | macOS / iOS |
|---|---|---|---|
| Initial TTL | 128 | 64 | 64 |
| TCP option order | MSS, NOP, WS, NOP, NOP, SACK | MSS, SACK, TS, NOP, WS | MSS, NOP, WS, NOP, NOP, TS, SACK, EOL |

It tells apart 3 families, not versions; Android and a Linux server look the same. An MSS below 1400 often means a tunnel, VPN or mobile network.

## How is the DNS path checked?

The DNS path is checked by opening a unique name like `a7f3k2….dns.caimancheck.com` through the proxy. The zone belongs to Caiman Check, so whoever resolves the name for the proxy has to ask our DNS server. The report compares that source with the exit IP: same network as the exit, public DNS, DNS in a datacenter, another network or unknown. Up to 30 attempts per provider, with constant checks that our own DNS path works.

## How are country, city and network type found?

Country and city come from the free DB-IP Lite database (CC BY 4.0), updated monthly: usually reliable for country, noticeably worse for city, especially on mobile networks. Network type is found from the network number and name (ASN): hosting, likely mobile or home ISP. It is accurate for big clouds; small regional networks can be misclassified.

## How are working lines counted?

A line is working if at least one of its requests succeeded — to a site or to Caiman Check's own check address. It is stable if all its site requests succeeded. Lines where nothing succeeded are listed by number with the main reason, ready to send to the provider. Sites where nobody succeeded are not counted against any line.

## How is IP diversity measured?

IP diversity is the number of different exit IPs across exit probes. Rotation versus sticky is detected by probing the same line again: if the IP stays the same on 70% of such lines, the proxies are sticky or static.

---

[← Back to README](../README.md) · Full methodology: [caimancheck.com/how](https://caimancheck.com/how)
