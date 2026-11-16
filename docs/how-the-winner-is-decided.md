# How does Caiman Check decide which proxy provider wins?

**Caiman Check names a winner only when a statistical test shows the difference is too big to be chance; otherwise it says the providers are about equal.** On two identical providers the verdict names a false winner in only 1–6% of runs (A/A simulation).

## Why not just compare the numbers?

Comparing raw numbers isn't enough because small samples are noisy: 5 out of 5 versus 2 out of 5 looks like a big gap but can easily be luck. Caiman Check reports "not enough data" in cases like that instead of a false winner.

## Which statistical tests are used?

Caiman Check uses a different test for each kind of metric:

- **Shares** (success, system, country, city, hosting IPs, load, UDP): Fisher's exact test at 95%, which stays correct on small numbers.
- **Success rate** must also hold within each site (Cochran–Mantel–Haenszel test), so one hard site can't create a false win. The difference is shown in percentage points with a 95% interval.
- **Speed**: the 95% bootstrap interval of the median difference must exclude zero and the gap must be at least 0.2 s. The faster provider also must not have more than 5 points more timeouts or fewer useful answers within 5 seconds.
- **Price**: compared only with at least 30 successful requests per provider; a gap under 15% is a tie, because providers' own traffic counters differ from ours by 5–15%.
- **Pool size**: compared by the effective-size estimate with its 95% interval, not by the number of IPs seen.
- **Secondary metrics** (system, hosting IPs, country, city, UDP, working lines, load) use a Holm correction for testing several at once; otherwise one of seven would "win" by chance.
- **Network diversity** is shown for information only, with no winner.

## How are repeated exit IPs handled?

Repeated exit IPs are not treated as new data, because requests through the same exit aren't independent. Probe-based shares (system, country, city, hosting IPs) are counted by distinct IPs, with no verdict under 8 distinct IPs. When exits repeat, the effective sample shrinks by **1 + (m − 1) × 0.1**, where m is requests per exit. Without this correction, pools of 10–50 exits gave 11–30% false wins in simulation; with it they stay around 5% or below.

## How is the method checked?

The method is checked by simulating two identical providers (an A/A test) through the same code that builds real reports. A release is blocked if any summary row names a false winner in more than 5% of runs. The same simulation checks that real differences are found: a provider 1.6× slower, with 35% hosting IPs and 10 points lower success, is called worse in 80–100% of runs.

## What does the final verdict say?

The verdict has two parts:

- **For antidetect:** the readiness score, if it differs significantly and all its significant parts favour the same provider; otherwise significant gaps in system, UDP, hosting IPs, country, city, success and working lines.
- **For scraping:** success, working lines, cost per 1,000 successful requests, load and speed.

With no significant gaps the verdict is "about equal". The table, the "key differences" block, the verdict and the PDF all use the same rules, so they always agree.

## How are key differences ranked?

Key differences are the up to 4 metrics with the largest gap, scaled by your scenario's weight. Gaps under 10% (15% for price) and success gaps within the margin of error are hidden.

| Metric | Antidetect | Scraping | General |
|---|---|---|---|
| Antidetect readiness | 1.5 | 0.5 | 1 |
| System match | 1.4 | 0.5 | 1 |
| UDP | 1.4 | 0.5 | 1 |
| Hosting IPs | 1.3 | 1.0 | 1 |
| Country | 1.2 | 0.9 | 1 |
| City | 1.1 | 0.8 | 1 |
| Success | 1.0 | 1.4 | 1 |
| Price of 1,000 successful | 0.8 | 1.5 | 1 |
| Speed | 0.7 | 1.2 | 1 |
| Load | 0.6 | 1.3 | 1 |

---

[← Back to README](../README.md) · Full methodology: [caimancheck.com/how](https://caimancheck.com/how)
