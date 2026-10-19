# Run

Run is one of [Pane](https://github.com/pane-app/pane)'s official
extensions, and it runs on Windows alone: its `pane.json` declares
`windows` only, so Pane installs it as a default extension on Windows and
never elsewhere. It runs what Windows' Run dialog (Win+R) runs — a
command line naming a program by its name or path with arguments, a
Control Panel applet, a management console, a document, a folder, a
network path, a `shell:` or `ms-settings:` address or another registered
scheme, with environment variables expanded — and shares the Run dialog's
own history in both directions: what ran in either appears in both, and
deleting an entry here removes it there.

"Run" is a no-view command that takes a query: it is offered as a
fallback for any text and usable through an alias, so a command line
typed in root search runs when Enter is pressed. "Run as Administrator"
runs the same command line through Windows' elevation prompt, which the
user may decline; "Run in Terminal" runs it where console tools stay
open to be read — in a new Windows Terminal tab when it is installed,
otherwise in the shell's own window, with the shell chosen in the
extension's preferences. "Run with Completions" opens a screen whose
search field holds the command line, completed as it is typed from the
Run dialog's history, programs in App Paths and on the search path,
Control Panel applets, management consoles, registered schemes and
environment variables. "Run History" lists the shared history, newest
first: Enter runs an entry again, and Delete removes it.

This repository holds the extension's whole package, in the one-extension
shape of [ADR 0044](https://github.com/pane-app/pane/blob/main/docs/adr/0044-a-git-repository-holds-one-extension-or-a-collection.md):
[`pane.json`](pane.json), the images it names, and the Rust source
([`src/`](src), [`Cargo.toml`](Cargo.toml)) of its WebAssembly component.
The [release tags](#releases) hold the built component beside the
manifest and the images.

## Install

Install it from Git with Pane alone — no Git, compiler or other tool is
needed on your computer. In root search, choose **Install extension from
Git…** and name

    https://github.com/pane-app/run@v0.3.0

or, from a terminal, `pane --install git:github.com/pane-app/run@v0.3.0`.
A `v<version>` tag pins a release: the tag's commit holds the package
complete, at the version `pane.json` names. Without a reference, Pane
installs the default branch, which between releases holds only the source
and is explained to users as source-only.

## Releases

A release is made from GitHub: **Actions → Release → Run workflow** on the
branch to release. The workflow builds the component with the same CI,
copies it where `pane.json` names it, commits that on the `release`
branch, tags the commit `v<pane.json's version>` and pushes the branch
and the tag. The tag's commit is the *release revision*: it holds
`pane.json`, the images and the built component (`run.wasm`), and it
is what Pane installs when a user names the tag — and the commit a Pane
release pins for its first setup (pane-app/pane#282).

To release a new version, raise `version` in `pane.json` (and in
`Cargo.toml`) through a pull request, then run the workflow. A tag is never
re-written: a released version that must change is released again as a new
version.

## Building

```
cargo build --release --target wasm32-wasip2
```

builds `target/wasm32-wasip2/release/run.wasm`; copy it to `run.wasm` at
the package root, the path its `pane.json` names, for a revision users
can install — what the Release workflow does. `rust-toolchain.toml` pins
the toolchain Pane builds its own extensions with.

The component builds against Pane's Rust SDK, `pane-extension`
(`pane-extension = "0.2"` in [`Cargo.toml`](Cargo.toml)). The SDK is not
published to crates.io yet (pane-app/pane#281); until it is, point a path
at a Pane checkout's `guests/pane-extension` — as this repository's CI
does — by appending to `Cargo.toml`:

```toml
[patch.crates-io]
pane-extension = { path = "<a Pane checkout>/guests/pane-extension" }
```

pane-app/pane#287 removes the CI patch once the crate is published.

## Issues and contributing

This extension's issues live in
[this repository's tracker](https://github.com/pane-app/run/issues).
The extension API — everything a Pane command can do, and how a package is
put together — is documented in Pane's repository:
[the extension guests' README](https://github.com/pane-app/pane/blob/main/guests/README.md)
and [Pane's docs](https://github.com/pane-app/pane/tree/main/docs).

## License

Licensed under either of [Apache-2.0](LICENSE-APACHE) or
[MIT](LICENSE-MIT), as Pane is: choose either at your option.
