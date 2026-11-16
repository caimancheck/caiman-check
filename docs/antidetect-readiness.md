# What is the antidetect readiness score for proxies?

**The antidetect readiness score shows in one number how well a proxy suits an antidetect browser: does the exit look like the chosen system, carry UDP, avoid datacenter IPs and sit in the promised country.** Caiman Check calculates it from four measured parts and always shows them next to the score.

## What is the score made of?

| Parameter | Weight | Input |
|---|---|---|
| System match | 35 | % of connections with the chosen system |
| UDP through the proxy | 25 | share of lines with confirmed UDP |
| No hosting / datacenter IPs | 25 | 100% minus their share |
| Country match | 15 | % of exits in the promised country |

## When is there no score?

There is no score when fewer than 3 of the 4 parts were measured, because a score from two parts would be misleading. A part missing for either provider is excluded for both, and the remaining weights are rescaled (without UDP: 47 / 33 / 20).

## Why doesn't a small MSS lower the score?

A small MSS (below 1400) doesn't lower the score because it appears both in tunnels and VPNs and in ordinary mobile networks, whose IPs are valuable for antidetect work. It is shown separately for information.

## What doesn't the score cover?

The score doesn't cover the browser fingerprint (canvas, WebGL, fonts, timezone) — that's the antidetect browser's job — or IP reputation in paid antifraud databases.

---

[← Back to README](../README.md) · Full methodology: [caimancheck.com/how](https://caimancheck.com/how)
