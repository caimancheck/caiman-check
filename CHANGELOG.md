# What changed in the Caiman Check methodology?

**The Caiman Check methodology changes when tests show a number could be wrong, and every change is logged here and on [caimancheck.com/how](https://caimancheck.com/how).** Reports made with an older version are marked as older methodology.

## v1.10 — comparison rules (15 Nov 2026)

Found by an A/A simulation of the whole report:

- Price in the summary table "won" on any gap, even a cent; now a gap under 15% is a tie, with one rule for the table, key differences and verdict.
- System, country, city and hosting shares were tested as if every probe were a new IP, giving up to 60% false wins with 12 exits; significance now counts distinct IPs.
- The readiness score could name a provider better even when its only significant difference was against it; now every significant part must favour it.
- Speed and pool size are compared with a chance check; network diversity has no winner; secondary metrics use a Holm correction.
- On identical providers the verdict was wrong in 20–66% of small-pool reports; now 1–5% (800 runs).
- The pre-test line check makes up to 5 tries instead of one.
- The "for antidetect" verdict also counts significant gaps in success and working lines.
- UDP is tested on every line instead of 5 random ones, with up to 3 tries; dead lines are not counted.
- Report colours appear only when a gap passed the chance test.

## v1.10 (14 Nov 2026)

Line assessment also counts the check request to our address, so a line whose 3 requests hit blocking sites is "working with interruptions", not dead. The test site list became configurable and each report shows which sites it used.

## v1.9 (Nov 2026)

"By site" table with error causes; sites where nobody succeeded aren't counted against lines; "unchecked" status for lines with no finished requests; copy buttons for working, dead and unchecked lines.

## v1.8 (13 Nov 2026)

After an external audit: Fisher's exact test instead of a z-test; a within-site check for success (Cochran–Mantel–Haenszel); success difference with a 95% interval; price and speed compared only from 30 successful requests each; a 503 without protection signs is a site error; correction for repeating exits; the "few devices" threshold raised from 5 to 8 IPs.

## v1.7 (12 Nov 2026)

The readiness score requires at least 3 of 4 measured parts. New "Proxy DNS path" check. Proxy type (residential or mobile) and the pre-run check with verdicts. New "Pool size" mode with effective-size estimate, calibration on pools with known truth, IPv6 /64 handling.

## v1.6 (11 Nov 2026)

Lists of up to 1,000 lines per provider with at least 3 requests per line; "Working lines" block; rotation detected by re-probing; internal-network proxy addresses rejected.

## v1.5 and earlier (11 Nov 2026)

TLS handshake estimate per connection; up to 20 redirects like Chrome; separate connect and response timeouts; User-Agent OS matches the chosen profile; current Chrome fingerprint; precise verification-page markers; traffic counted before decompression (earlier "price of 1,000 successful requests" was several times too high); separate request queues for A and B.
