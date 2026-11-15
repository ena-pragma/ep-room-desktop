# EP Room Desktop releases

Binaries only. The source lives in a private repository; this repository holds the public releases that EP Room Desktop
updates itself from, and the one-line installer.

## Install or update

Any Mac, Windows or Linux machine with Node 18+:

```sh
npx -y github:ena-pragma/ep-room-desktop
```

On a machine with the EP Room CLI:

```sh
ep-room desktop update            # --check to only look, --force to reinstall
```

It downloads the newest release for this machine, checks its sha256 against `SHA256SUMS` in the release, and installs it.
On a Mac it replaces `EP Room Desktop.app` in `~/Applications` (or `/Applications`), keeping the old one in
`~/Applications/_rollback`; if the app is open it is never quit: the update installs the next time you quit it.

## Signing

- macOS builds are ad-hoc signed and not notarized until a Developer ID certificate exists. The command above clears the
  download quarantine. Opening a dmg from this page by hand: right-click the app, Open, the first time.
- Windows installers are unsigned (`UNSIGNED-TEST` in the name): SmartScreen asks once ("More info", "Run anyway").
- The app checks this feed at launch and every 6 hours. Windows and Linux (AppImage) update themselves and install on
  quit; an ad-hoc Mac build tells you to run `ep-room desktop update`.

## How a release is made

`release/SOURCE.json` names a version and a source commit. A change to it (pushed by the source repository's
`desktop-release` workflow when `electron/package.json` has a new version) runs `.github/workflows/release.yml`: it builds
macOS (arm64 and x64), Windows (x64) and Linux (AppImage, deb) from that commit and publishes `desktop-v<version>` with
`latest-mac.yml`, `latest.yml`, `latest-linux.yml` and `SHA256SUMS`. Everything in this repository except
`release/SOURCE.json` is copied from the source repository's `electron/release-repo/` and `bin/ep-room-desktop`; edit it there.
