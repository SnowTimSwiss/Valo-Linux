# valo-release

`valo-release` installs Valo release metadata and a Valo APT source template.

In `v0.1`, the package installs:

- `/etc/valo/version`
- `/usr/share/valo-release/valo.sources.template`

The `v0.1` APT source template uses `Trusted: yes` because the production
signing key does not exist yet. Enable it only once the GitHub Pages repository
has published matching Trixie metadata.
