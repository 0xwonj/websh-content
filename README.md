# websh-content

Public authored content for [websh](https://github.com/0xwonj/websh). The app is
published separately on IPFS. This repository contains profile/Now, formal articles,
papers and other media, public ACK commitments, and external mount declarations.
[websh-mempool](https://github.com/0xwonj/websh-mempool) remains an independent unsigned
draft source mounted at `/mempool`.

## Everyday updates

Install the CLI once from the app checkout:

```bash
cargo install --locked --path crates/websh-cli
```

Edit `content/.site/now.toml`, `content/.site/profile.toml`, or other authored content,
then run from this repository:

```bash
websh-cli publish
```

The command generates and validates metadata, explicitly signs the root manifest with
the owner's local GPG key, creates a frozen content commit and pointer commit, and
pushes once. No app build, IPFS upload, ENS transaction, or deployment credentials are
involved. The browser resolves `current.json` and reads only its selected immutable
commit. Public visibility can lag a successful push; rerun the command to check again
without issuing another unchanged release.

Keep the Git index unstaged before publishing. Push initial setup and later README/CI
commits through ordinary Git first; only a prepared snapshot/pointer pair may remain
ahead of origin. A rejected push leaves that pair available for retry. Set newer edits
aside until it is published. If origin advanced, reconcile authored inputs onto the
fetched branch and publish again; never rebase the prepared pair or force push published
history. Historical content links depend on retained commits.

## Sources and generated files

- Markdown metadata lives in frontmatter.
- Binary metadata lives in `file.ext.meta.json`.
- `_index.dir.json` declares directories, bundles, and explicit publication groups.
- `content/.websh/mounts/*.mount.json` declares independent unsigned GitHub sources.

`content/manifest.json`, `content/manifest.sig`, `content/.websh/ack.commitment.json`,
`content/.websh/attestations.json`, and `current.json` are generated. Do not edit them
by hand. Optional portable article proofs use `websh-cli attest`; the whole root
snapshot is always authenticated by its own detached manifest signature.

```bash
websh-cli sync                       # Generate without signing or network access.
websh-cli sign                       # Explicitly issue/sign the root manifest.
websh-cli check --require-signatures # Read-only validation, suitable for CI.
```

Nothing private belongs in `content/`. ACK source names/nonces stay in ignored
`.websh/local/crypto/`; secret signing keys stay in local GPG. CI uses only public
artifacts, checks a pinned CLI implementation, and never receives owner keys.

Detailed authoring and command contracts live in the
[maintained CLI guide](https://github.com/0xwonj/websh/blob/main/docs/architecture/cli.md).
