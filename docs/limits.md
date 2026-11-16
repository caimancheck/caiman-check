# What can't a proxy test measure?

**A proxy test can't tell you everything: it shows how proxies behave for an HTTP client, at one moment, from one server.** Caiman Check lists its limits openly so you know how far to trust each number.

## What doesn't Caiman Check test?

- **Browser fingerprint** (canvas, WebGL, fonts, timezone): that's the antidetect browser's job.
- **Real WebRTC leaks in your browser:** Caiman Check tests whether the proxy can carry UDP.
- **Availability history:** the test shows the state at test time.
- **IP reputation** in paid antifraud databases.
- **IPv6 exits:** the check address is IPv4-only, so system, country and network describe IPv4 exits.
- **JavaScript challenges:** requests don't run JavaScript, so a real browser can pass checks an HTTP client stops at, and the other way round.

## When is a comparison unreliable?

- **Few devices:** with fewer than 8 different exit IPs, 100 requests come from a few devices; the report warns and names no success winner.
- **Different modes:** if one provider is sticky and the other rotates, IP diversity isn't compared.
- **Different setup:** the form warns if A and B differ in protocol, country or number of lines.
- **One run:** residential pools change; "Repeat with the same settings" builds a series showing how often each provider won.
- **Server location:** tests run from Europe, so European entry servers will be faster than American ones. Conditions are equal for A and B, but absolute seconds elsewhere would differ.
- **Price:** traffic is what our server sees; check absolute dollars against your provider's dashboard.

## What happens to my data?

- Proxy credentials exist only in memory during the test and are never written to the database, files or logs.
- A report is visible only to the person who ran it; reports and PDFs contain no credentials.
- Exit IPs are not stored. In "Pool size" mode IP hashes (HMAC) are kept for 48 hours to combine runs, then deleted.
- Proxies with internal-network addresses (localhost, 10.x, 192.168.x) are not tested.

---

[← Back to README](../README.md) · Full methodology: [caimancheck.com/how](https://caimancheck.com/how)
