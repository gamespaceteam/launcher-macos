# Game Space Launcher — macOS releases

Apple Silicon (arm64) release artifacts for Game Space Launcher.

Source code and history live in the private [Gitea repository](https://gitea.gamespace.team/loshadka/gs-launcher-macos).
This repository contains only the GitHub Actions workflow, release documentation and packaged releases.

Run **Actions → Build macOS launcher → Run workflow** with an exact Gitea source commit SHA.
The workflow uses a read-only deploy key, runs tests, builds DMG/ZIP on an Apple Silicon runner,
smoke-tests the packaged application and creates a **draft** GitHub release.

Without Apple Developer ID, leave `signed` disabled. Test builds use ad-hoc signing and disable launcher auto-update.
They are not production installers and have not been notarized by Apple.

For signed/notarized builds configure repository secrets:

- `GITEA_SSH_KEY`: read-only key for the single source repository.
- `MAC_CERTIFICATE_P12`: base64 Developer ID Application certificate including its private key.
- `MAC_CERTIFICATE_PASSWORD`: P12 export password.
- `APPLE_ID`, `APPLE_APP_SPECIFIC_PASSWORD`, `APPLE_TEAM_ID`: notarization credentials.

Review and publish a draft only after installation has been verified on a real Mac.
Increase the version in the source repository for each released launcher update.
Game builds are distributed independently through the game CDN.
