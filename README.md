# Caiman Check — compare two proxy providers side by side

**To compare two proxy providers, give both the same job at the same time and see which one opens more pages, works faster and costs less per page that worked.** Caiman Check does this for you: paste proxies from two providers and get a report in a few minutes, with a winner named only when the difference is too big to be luck.

🌐 **Try it free:** [caimancheck.com](https://caimancheck.com) · 📐 **Full methodology:** [caimancheck.com/how](https://caimancheck.com/how) · 📰 **Articles:** [caimancheck.com/articles](https://caimancheck.com/articles)


This repository is the public home of Caiman Check: how it tests proxies, how every number in a report is calculated, and what changed between methodology versions. The service itself runs at [caimancheck.com](https://caimancheck.com).

---

## What is Caiman Check?

Caiman Check is a free web tool that tests two residential or mobile proxy providers on the same sites, at the same time, from the same server, and tells you which one is better for your task. It can also check a single provider's proxy list and estimate how big a rotating proxy pool really is.

## How do I use Caiman Check?

You use Caiman Check by pasting proxy lines from two providers into the form at [caimancheck.com](https://caimancheck.com) and pressing start. Before the run it checks that the lines work and are really residential or mobile; then it sends the same requests through both providers and builds a report.

## What does a Caiman Check report show?

A Caiman Check report shows which provider opened more pages, which was faster and which was cheaper per page that worked. In detail:

| Metric | What it tells you |
|---|---|
| **Success rate** | Share of requests that returned the real page, with a reason for every failure |
| **Response time** | Median and slowest 10%, split into proxy connection, site response and download |
| **Price of 1,000 successful requests** | What you really pay per result, not per GB |
| **Working lines** | Which lines of your list work, with numbers of dead lines to send back to the provider |
| **Country, city, network type** | Where exits really are and whether they are home, mobile or datacenter IPs |
| **System match** | Whether the exit device's TCP fingerprint matches the OS you chose |
| **UDP** | Whether the proxy carries UDP (needed for WebRTC in antidetect browsers) |
| **DNS path** | Who resolves site names for the proxy |
| **Antidetect readiness** | One score from system match, UDP, hosting IPs and country |
| **Pool size** | Effective size of a rotating pool (separate mode) |

## How does Caiman Check decide which provider is better?

Caiman Check names a winner only when a statistical test shows the difference is real, not chance. Shares are compared with Fisher's exact test, success also within each site (Cochran–Mantel–Haenszel), speed with a bootstrap interval, and secondary metrics with a Holm correction. Repeated exit IPs are counted once, and price gaps under 15% are a tie. On two identical providers the verdict names a false winner in only 1–6% of runs.

→ [How the winner is decided](docs/how-the-winner-is-decided.md)

## Is Caiman Check free?

Caiman Check is free to use during its beta at [caimancheck.com](https://caimancheck.com).

## Who is Caiman Check for?

Caiman Check is for anyone who pays for residential or mobile proxies and wants proof they picked the right provider: web scraping and data teams, media buying teams, antidetect browser users and proxy resellers.

## Does Caiman Check store my proxy passwords?

No, Caiman Check never stores proxy logins or passwords. They live only in memory during the test and never reach the database, logs, PDFs or shared links. Exit IPs are not stored either.

---

## Documentation

Every page answers one question. The source of truth is [caimancheck.com/how](https://caimancheck.com/how); these pages follow methodology **v1.10 (November 2026)**.

- [How does a Caiman Check test run?](docs/how-a-test-runs.md)
- [What counts as a successful proxy request?](docs/what-counts-as-success.md)
- [How is each metric calculated?](docs/metrics.md)
- [How does Caiman Check decide the winner?](docs/how-the-winner-is-decided.md)
- [What is the antidetect readiness score?](docs/antidetect-readiness.md)
- [How do you estimate the size of a rotating proxy pool?](docs/pool-size.md)
- [What can't a proxy test measure?](docs/limits.md)
- [What changed between methodology versions?](CHANGELOG.md)

## How can I report a problem or suggest a test?

You can report a problem with a report or suggest a metric by opening an [issue](https://github.com/caimancheck/caiman-check/issues) or writing through [caimancheck.com/feedback](https://caimancheck.com/feedback). Please include the report number, never your proxy credentials.

---

Documentation is licensed under [CC BY 4.0](LICENSE): you can quote and reuse it with a link to Caiman Check.
