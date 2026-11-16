# How do you estimate the size of a rotating proxy pool?

**You estimate a rotating proxy pool by sending many requests through one line and counting how often the same exit IP comes back: the fewer repeats, the bigger the pool.** Caiman Check's "Pool size" mode does this and gives the effective pool size with a 95% interval.

## Why can't a provider fake the result?

A provider can't fake the result because requests go over HTTPS to Caiman Check's own server, which sees the real exit address. Each request is about 7 KB. An exit is an IPv4 address or an IPv6 /64.

## What numbers does the report give?

- **Effective pool size now:** N = 1/Σp², where p is the chance of getting a given exit. It equals the number of exits when rotation is even, and is smaller when some exits come up more often. Estimate: n(n−1)/2C, where C is the number of request pairs with the same exit. The 95% interval uses Poisson for C, widened by the measured overdispersion.
- **Total exits, at least:** a lower bound only — the larger of Chao1 and the effective-size lower bound. The true total can't be known from outside when issuance is uneven.
- **Similarity between runs:** Σpᵢpⱼ/√(Σpᵢ²·Σpⱼ²), from 0 to 1, plus containment both ways ("run 1 inside run 2: 100%, run 2 inside run 1: 25%").
- **Effective size over a period:** the same formula over all runs in a series.

## What corrections are applied?

- **Sticky issuance:** if nearby requests repeat 5+ times more often than random issuance allows, those requests are removed so the estimate isn't distorted.
- **No-reuse window:** if no repeat appears closer than n/20 requests though 5+ were expected, only pairs beyond that window count.
- **IPv6:** exits are counted per /64; the report shows IPv4 and /64 separately and how the count changes when grouping by /56 or /48.

## How accurate is it?

The method was checked on simulated pools with known size, 100 runs per scenario. The 95% interval contained the true size in 94–99% of runs, including 1M and 15M exits, uneven load, sticky issuance and no-reuse windows. For 15M exits and 55,000 requests the median estimate was 14.9M.

## How many requests do I need?

You need about √(200·N) requests for roughly 100 repeats: about 15,000 for a 1M pool (≈100 MB) and about 55,000 for a 15M pool (≈380 MB, 20–30 minutes).

## What can't be known from outside?

You can't know from outside how many devices share one IP, how many exits other customers get, or how big the pool was on other days. Provider figures like "15M IPs" usually count all IPs over a month.

---

[← Back to README](../README.md) · Full methodology: [caimancheck.com/how](https://caimancheck.com/how)
