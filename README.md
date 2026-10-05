# MODI PS3 Flash

GitHub Pages service copy of [xXEvilnatXx/flash-writer](https://github.com/xXEvilnatXx/flash-writer), pinned to commit `a167c406d059e168bbdb70a067a5fe35b84bcb19` (Flash Writer 4.93, unofficial).

Site: https://modyfikatorcasper.github.io/modi-ps3-flash/

The original `index.html` and `flash493.P3T` are copied byte-for-byte into `writer/`. Original authors and credits remain intact. The root index adds only a separate access-code screen, using old-browser JavaScript and ordinary form controls for the PS3 browser.

## Access limitation

The access-code screen is a convenience gate, not authentication. All GitHub Pages assets and this repository are public. The code check can be inspected, the six-digit code can be recovered, and the writer can be reached directly without logging in. No sensitive data should be stored here. The noindex tag asks search engines not to index the login screen; it does not restrict access.

## Deployment and validation

Publish branch `main`, folder `/ (root)`, from Settings > Pages. The `.nojekyll` file keeps these static assets unprocessed.

Verify successful and rejected login attempts, then test loading and operation separately on a compatible PS3. Desktop checks do not establish PS3 compatibility or validate writing console flash. Do not redesign the original writer before the initial console test.
