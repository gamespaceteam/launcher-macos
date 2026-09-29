# Game Space Launcher — macOS releases

Apple Silicon (arm64) release artifacts for Game Space Launcher.

Source code and history live in the private [Gitea repository](https://gitea.gamespace.team/loshadka/gs-launcher-macos).
This repository contains only the GitHub Actions workflow, release documentation and packaged releases.

Run **Actions → Build macOS launcher → Run workflow** with an exact Gitea source commit SHA.
The workflow uses a read-only deploy key, runs tests, builds DMG/ZIP on an Apple Silicon runner,
smoke-tests the packaged application and creates a **draft** GitHub release.

Without Apple Developer ID, leave `signed` disabled. This is the default distribution mode: builds use local ad-hoc signing and do not require an Apple account. They have not been notarized by Apple.

Download the DMG, move the app into Applications and open it. If macOS blocks it as an unidentified developer, use **System Settings → Privacy & Security → Open Anyway** after the first launch attempt, as described in [Apple's instructions](https://support.apple.com/102445). Verify this on a real Mac after a browser download; CI does not reproduce browser quarantine.

Starting with **0.1.2**, launcher updates download automatically from the latest published GitHub release. Restart when prompted to install, or use **Settings → Launcher updates**. The updater verifies an Ed25519-signed manifest, SHA-256, bundle identity and version, then atomically replaces the app and rolls back if the new launcher fails to start. Installed games and settings remain in Application Support. Versions 0.1.0/0.1.1 need one manual upgrade to 0.1.2.

Install into a writable Applications folder (or `~/Applications`), not the DMG. No administrator elevation, Gatekeeper disabling or automatic quarantine removal is performed. Draft/prerelease builds are not served by the updater. To deliver an update, increase the version, build it and publish its draft **as the latest release**, including `launcher-update.json` and the ZIP. Keep the same update signing key across releases.

When Developer ID becomes available, configure the secrets below and enable `signed`. The update manifest is still signed independently. Test that transition when the certificate is available; installed Developer ID builds will enforce the same Apple Team ID for subsequent updates.

For signed/notarized builds configure repository secrets:

- `GITEA_SSH_KEY`: read-only key for the single source repository.
- `LAUNCHER_UPDATE_PRIVATE_KEY`: Ed25519 PEM signing key for automatic updates, already configured. Its public counterpart is embedded in the launcher. Do not replace independently of a client trust-migration plan.
- `MAC_CERTIFICATE_P12`: base64 Developer ID Application certificate including its private key.
- `MAC_CERTIFICATE_PASSWORD`: P12 export password.
- `APPLE_ID`, `APPLE_APP_SPECIFIC_PASSWORD`, `APPLE_TEAM_ID`: notarization credentials.

Review and publish a draft only after installation has been verified on a real Mac.
Increase the version in the source repository for each released launcher update.
Game builds are distributed independently through the game CDN.
