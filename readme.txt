=== scrt.link for WordPress ===
Contributors:      mikezielonka
Tags:              block, encryption, privacy, secrets, scrt.link
Requires at least: 6.6
Tested up to:      6.9
Requires PHP:      7.4
Stable tag:        0.1.5
License:           GPLv2 or later
License URI:       https://www.gnu.org/licenses/gpl-2.0.html

Drop a block onto any page to let visitors send you end-to-end encrypted, self-destructing secrets via scrt.link.

== Description ==

`scrt.link for WordPress` ships a single Gutenberg block — **Send me a secret** — that turns any WordPress page into a personal, end-to-end encrypted drop-box powered by [scrt.link](https://scrt.link).

* **End-to-end encrypted.** Encryption happens in the visitor's browser using scrt.link's client crypto module. Your server and WordPress never see plaintext.
* **One-time.** Each submission becomes a self-destructing scrt.link URL delivered to your notification email.
* **White-label friendly.** Point the plugin at your own scrt.link deployment or stick with `https://scrt.link`.
* **Modern stack.** `apiVersion: 3`, Interactivity API (`viewScriptModule`), dynamic server-rendered block — ready to migrate to `@wordpress/build` when it stabilizes for single-block plugins.

== Installation ==

1. Upload the plugin files to `/wp-content/plugins/scrt-link-wp/` or install via the Plugins screen.
2. Activate the plugin.
3. Go to **Settings → scrt.link** and paste your scrt.link API key. Optionally configure a white-label base URL and notification email.
4. Add the **Send me a secret** block to any page.

== Frequently Asked Questions ==

= Does WordPress see the secret contents? =

No. The visitor's browser encrypts the payload before it's sent to WordPress. WordPress forwards the ciphertext to scrt.link, authenticated with your API key, and emails the resulting self-destructing URL to your notification address.

= Where is my API key stored? =

In the `wp_options` table (site-level option), only readable by users with `manage_options`. It is never emitted in block markup or sent to the browser.

= Can I use a self-hosted scrt.link instance? =

Yes. Set the base URL in **Settings → scrt.link** to your deployment.

== Changelog ==

= 0.1.5 =
* Drop WP cookie-based nonce auth for the submit endpoint. Managed-WP hosts + CDN layers (BigScoots, Cloudflare) strip or rewrite auth cookies on `/wp-json` POSTs, so WP nonce verification would fail for real visitors with "Cookie check failed" no matter how fresh the nonce was. The endpoint is now protected by: (1) Origin-header check (rejects cross-origin POSTs), (2) per-IP rate limit, (3) end-to-end encryption of the payload itself, (4) the scrt.link API key never leaving PHP.

= 0.1.4 =
* Attempted fix for "Cookie check failed" by refetching nonces from an uncached REST endpoint. Turned out insufficient — the CDN layer interferes with cookies on `/wp-json` POSTs regardless. Superseded by 0.1.5.

= 0.1.0 =
* Initial release.
