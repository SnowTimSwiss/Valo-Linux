# Valo Packages

This directory is reserved for small Valo-owned Debian package sources.

These packages are the planned update path for Valo-specific changes after
installation. They should stay tiny and focused.

## Planned Packages

- `valo-keyring`: installs the public signing key for the Valo APT repository.
- `valo-release`: installs Valo release metadata and the Valo APT source template.
- `valo-defaults`: installs Valo defaults and handles safe migrations.

The Valo package repository should not mirror Debian packages. It exists only
for Valo-owned integration packages.

## Future Layout

Each package should eventually use a normal Debian package layout:

```text
packages/valo-defaults/
  debian/
    changelog
    control
    install
    postinst
```

Do not add package contents until the corresponding Valo behavior is ready to
be supported on installed systems.
