# What counts as a successful proxy request?

**A proxy request is successful when the site returns the real page you asked for — not a block, a captcha or an empty stub.** Caiman Check checks this with a marker of the expected page on every site, so a "200 OK" login wall doesn't count as success.

## Why isn't status 200 enough?

Status 200 isn't enough because a site can return a login wall, a placeholder or an "enable JavaScript" page with status 200. Each site in Caiman Check has a marker of the expected page; without it the request fails as "wrong page". If no provider ever got the expected page from a site, the site probably changed its markup: it is excluded and the report says so.

## What counts as a block?

A block is a status 403 or 429, a 503 with protection signs, or a verification page. Verification pages are recognised only by precise markers, not single words, because normal pages also contain words like "recaptcha":

- Cloudflare: header `cf-mitigated: challenge`, `cf_chl_opt`, `cf-chl-widget`, title "Just a moment..."
- Google: `/sorry/`, "unusual traffic"
- Amazon: `/errors/validateCaptcha`
- DataDome: `captcha-delivery.com`
- PerimeterX: `px-captcha`
- Akamai: title "Access Denied", `errors.edgesuite.net`
- Reddit: "whoa there, pardner"
- AWS WAF: `AwsWafIntegration`

A 503 without protection signs counts as a site error, not a block. Redirects are followed like Chrome (up to 20).

## Why did a request fail?

Every failed request gets a cause, so you can tell a proxy problem from a site problem.

| Report category | What happened technically |
|---|---|
| Wrong proxy login or password | SOCKS5 rejected auth, or HTTP proxy returned 407 |
| Proxy server not responding | Couldn't connect to the proxy itself |
| Provider blocks this site | SOCKS5 reply code 2, or HTTP CONNECT 403/451 |
| Exit IP couldn't reach the site | Proxy accepted, but the exit device failed to connect (SOCKS5 codes 1, 3–6, CONNECT 5xx) |
| Couldn't connect within 15 seconds | Connection timed out |
| Full answer took over 15 seconds | Operation timed out |
| Site certificate failed verification | Through a proxy, a possible sign of traffic interception |
| Secure connection error | TLS handshake failure, often a channel dropped mid-handshake |
| Connection dropped | Connection closed mid-response |
| Site blocked it | Captcha, 403, 429 or 503 with protection signs |
| Site returned an error | Other 4xx and 5xx from the site |

---

[← Back to README](../README.md) · Full methodology: [caimancheck.com/how](https://caimancheck.com/how)
