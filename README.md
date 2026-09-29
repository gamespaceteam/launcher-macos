# Game Space Launcher — macOS releases

Apple Silicon (arm64) release artifacts for Game Space Launcher.

Source code and history live in the private [Gitea repository](https://gitea.gamespace.team/loshadka/gs-launcher-macos).
This repository contains only the GitHub Actions workflow, release documentation and packaged releases.

Run **Actions → Build macOS launcher → Run workflow** with an exact Gitea source commit SHA.
The workflow uses a read-only deploy key, runs tests, builds DMG/ZIP on an Apple Silicon runner,
smoke-tests the packaged application and creates a **draft** GitHub release.

Without Apple Developer ID, leave `signed` disabled. This is the default distribution mode: builds use local ad-hoc signing and do not require an Apple account. They have not been notarized by Apple.

Download the DMG, move the app into Applications and open it. If macOS blocks it as an unidentified developer, use **System Settings → Privacy & Security → Open Anyway** after the first launch attempt, as described in [Apple's instructions](https://support.apple.com/102445). Verify this on a real Mac after a browser download; CI does not reproduce browser quarantine.

Launcher updates are manual: open **Settings → Launcher updates → Open releases page**, download a newer DMG, quit the launcher from its menu, and replace the application. Installed games and settings remain in Application Support. Game downloads and updates still run inside the launcher, and ad-hoc game bundles do not require an Apple Team ID.

When Developer ID becomes available, configure the secrets below and enable `signed`. Install that first signed launcher manually; subsequent signed releases can use automatic updates. The source README documents independent signing policies for the launcher and game.

For signed/notarized builds configure repository secrets:

- `GITEA_SSH_KEY`: read-only key for the single source repository.
- `MAC_CERTIFICATE_P12`: base64 Developer ID Application certificate including its private key.
- `MAC_CERTIFICATE_PASSWORD`: P12 export password.
- `APPLE_ID`, `APPLE_APP_SPECIFIC_PASSWORD`, `APPLE_TEAM_ID`: notarization credentials.

Review and publish a draft only after installation has been verified on a real Mac.
Increase the version in the source repository for each released launcher update.
Game builds are distributed independently through the game CDN.
