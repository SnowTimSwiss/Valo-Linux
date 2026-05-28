# Valo Update Model

Valo is based on Debian 13 (Trixie) and should stay close to Debian. Most packages
come directly from Debian, and users update those packages with normal APT
commands.

Valo still needs a way to deliver Valo-specific changes after installation. For
example, `v0.2` may need to adjust a configuration file that was installed by
`v0.1`. Debian alone cannot deliver that change, because Debian does not
know about Valo's custom defaults.

## Decision

Valo will maintain a very small APT repository in this GitHub repository. The
repository will be published through GitHub Pages. During `v0.1` this
repository is trusted through HTTPS only; for public non-test releases it should
be signed with a Valo archive key.

This repository is only for Valo-owned integration packages:

- `valo-keyring`: installs the public signing key for the Valo APT repository.
- `valo-release`: installs Valo release metadata and the Valo APT source file.
- `valo-defaults`: installs Valo defaults and handles safe migrations.

Valo will not mirror Debian packages and will not fork Debian's package archive.

## v0.1 Valo Repository

For early releases, `valo-release` enables this repository URL:

```text
https://snowtimswiss.github.io/Valo-Linux
```

The `v0.1` source uses `Trusted: yes` because the production archive signing key
does not exist yet. This lets installed `v0.1` systems receive `valo-defaults`
`0.2` later through normal APT, but it is not the final security model.

Before a public non-test release, replace `Trusted: yes` with:

```text
Signed-By: /usr/share/keyrings/valo-archive-keyring.gpg
```

and ship the matching public key through `valo-keyring`.

## How Updates Reach Users

An installed Valo system should have both Debian 13 (Trixie) sources and the Valo
APT source configured.

Users update with the standard workflow:

```sh
sudo apt update
sudo apt upgrade
```

When Valo `v0.2` needs to change something from `v0.1`, the change should be
published as a new version of one of the Valo packages, usually
`valo-defaults`.

For example:

```text
valo-defaults 0.1
valo-defaults 0.2
```

The Debian package maintainer scripts can migrate existing systems carefully:

```sh
if [ -n "$2" ] && dpkg --compare-versions "$2" lt "0.2"; then
    # migrate older Valo installations
fi
```

Migration scripts must be idempotent. Running them twice should not damage the
system.

## Repository Hosting

The same GitHub repository can host both the source and the published APT
repository:

```text
main branch
  config/                  live-build configuration
  packages/                Valo package sources
  .github/workflows/       automation

gh-pages branch
  dists/                   published APT metadata
  pool/                    published .deb files
```

GitHub Actions should build the packages and publish the repository to the
`gh-pages` branch. Once the signing key exists, the workflow can sign the
repository metadata with a GPG key stored in GitHub Secrets.

## Security Requirements

Before a public non-test release:

- Generate an offline GPG signing key for the APT repository.
- Store the private key as a GitHub Actions secret.
- Ship the public key through `valo-keyring`.
- Configure the APT source with `Signed-By=`.
- Never publish unsigned repository metadata.

## First Packages

The initial package set should stay intentionally small:

```text
valo-keyring
valo-release
valo-defaults
```

Avoid adding packages for preferences that can remain in Debian, upstream
projects, or documentation.
