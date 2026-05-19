# valo-keyring

`valo-keyring` will install the public signing key for the Valo APT repository.

The package is intentionally empty in `v0.1` because the real repository signing
key has not been generated yet. Do not ship a placeholder trust key.

Once the real key exists, add it as:

```text
usr/share/keyrings/valo-archive-keyring.gpg
```

Then install it from `debian/install`.
