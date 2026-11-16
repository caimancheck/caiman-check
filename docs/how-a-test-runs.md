# How does a Caiman Check test run?

**A Caiman Check test sends the same requests through two proxy providers at the same time, to the same sites, with the same settings, and counts what happened to each request.** Nothing is retried, so the report shows the real error rate.

## What happens before the test?

Before the test Caiman Check checks every provider's lines for free, so you don't waste a run on broken or wrong proxies.

- **Lines.** One request through each of the first 10 lines to our own address, which returns the caller's IP. A residential exit can vanish for a few seconds, so a line gets up to 5 tries with a 1.5 s pause before it counts as not answering. Wrong logins and provider blocks are not retried.
- **Rotation.** 6 requests in a row through the first line show whether the IP changes.
- **Login settings.** If the login sets a country, city, ISP, system or session (for example `country-de`, `session-abc`), Caiman Check shows it and compares it with the real exits.
- **Gateway.** The country of the proxy entry server and the connect time to it, because speed also depends on that distance.

The result is a green, yellow or red verdict. **Red** means 60% or more of the exit IPs (out of at least 3) belong to hosting providers — these are datacenter proxies, and the comparison would show nothing. **Yellow** warns about some hosting IPs, a different exit country or no mobile carriers for the "Mobile" type.

## Which sites does the test use?

The test uses a fixed set of real sites chosen for your scenario.

- **Scraping:** Amazon, eBay, Walmart and Etsy search, Booking hotel search, Bing results, a Wikipedia article.
- **Antidetect and accounts:** sign-up pages of Facebook, Instagram, TikTok, Google, Reddit, Discord, eBay and LinkedIn.
- **General:** home pages of popular sites plus Bing and DuckDuckGo results.

You can also test your own site and give text that must appear on the right page. Every report lists the sites it was measured on. Google search is not used: it needs JavaScript and returns no results without a browser.

## How are requests sent?

Requests are sent in turn through A and B: A's i-th and B's i-th request go to the same site.

- 12 requests at a time per provider (24 for tests of 600+ requests), in separate queues so a slow provider can't take threads from the other.
- Each line gets an equal share and at least 3 requests to different sites (up to 1,000 lines per provider).
- Timeout: 15 seconds per request. Each request uses a fresh connection.
- Each site is first opened once directly, without a proxy. Sites that don't answer at all are excluded; sites that block stay in, because that's where proxies differ.

## Do requests look like a real browser?

Yes, requests copy a real browser's TLS and HTTP/2 fingerprint (current Chrome, or Safari for iOS), and the User-Agent matches the OS you chose. They are not a full browser: JavaScript doesn't run and cookies are not kept. Conditions are equal for A and B, but absolute percentages for browser work can differ.

## What extra checks run during the test?

During the test some requests go to Caiman Check's own address to learn each exit's IP, country, city, network and system.

- Up to 20 lines: every fifth round over all lines. From 20 lines: each line once, and the first 10 lines a second time to see whether a line keeps its IP.
- UDP is tested on every line of the list.
- **Load mode** (optional): 3 steps of 10, 40 and 100 concurrent requests per provider. If Caiman Check's own server CPU reaches 85%, that step is excluded, because the limit was ours, not the proxy's.

---

[← Back to README](../README.md) · Full methodology: [caimancheck.com/how](https://caimancheck.com/how)
