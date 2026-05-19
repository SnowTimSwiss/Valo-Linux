# valo-release

`valo-release` installs Valo release metadata and the active Valo APT source
definition.

In `v0.1`, the package installs:

- `/etc/valo/version`
- `/etc/apt/sources.list.d/valo.sources`
- `/usr/share/valo-release/valo.sources.template`

The `v0.1` APT source uses `Trusted: yes` so early installed systems can update
from the GitHub Pages repository before the production signing key exists. Move
this to `Signed-By=/usr/share/keyrings/valo-archive-keyring.gpg` once the real
archive key is generated and shipped by `valo-keyring`.
