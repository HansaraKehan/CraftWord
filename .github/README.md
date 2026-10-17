<img width="2304" height="1440" alt="Screenshot_20261016_160056" src="https://github.com/user-attachments/assets/0e96585f-3dda-4ea9-8cd8-97434c9d3d19" />

[github/downloads/HansaraKehan/CraftPhoto/total](https://img.shields.io/github/downloads/HansaraKehan/CraftWord/total)

# WordCraft for Android (early preview)

An unofficial Android build of [WordCraft](https://github.com/storytold/wordcraft), the open-source word
processor written in Rust by the ArtCraft team. It reads and writes `.docx` and keeps the ribbon, styles,
tables, track changes and references of the desktop app. This build runs it as a native Android app (egui + wgpu),
not as a web page.

> **Early preview.** This is a first working port, not a finished product. See [Limitations](#limitations).

This repository does not contain WordCraft's source. A GitHub Actions workflow downloads the upstream source,
adds a small Android layer, builds the APK and publishes it under [Releases](https://github.com/HansaraKehan/CraftWord/releases).

## Download

Get the latest `.apk` from the [Releases](https://github.com/HansaraKehan/CraftWord/releases) page, open it and allow the install when Android asks.
You need:

- an ARM64 phone or tablet (most devices from the last several years)
- Android 9 or newer
- a GPU with Vulkan or OpenGL ES 3

No storage permission is needed. File ▸ Open and File ▸ Save As use Android's own file picker, so you can pick
documents anywhere: internal storage, an SD card, or cloud apps such as Google Drive.

WordCraft works on a private copy of the document you pick and writes every save back to the original location
within a couple of seconds. If the source is read-only (for example an email attachment), use **Save As** to choose a
place you can write to.

You can also open a document from another app: choose **WordCraft** in the "Open with" list for `.docx`, `.doc`, `.odt`,
`.rtf` and plain text files.

If an update won't install over an older build, uninstall the old one first.

## Limitations

- A hardware keyboard is recommended. Typing with the on-screen keyboard may be unreliable.
- Preferences are not saved between launches.
- No printing, and no clipboard images or formatted paste yet.
- The interface is the desktop layout; it is not redesigned for small touch screens.

## How builds are made

- The workflow in `.github/workflows/wordcraft-android-release.yml` runs every day and builds when upstream has a new commit.
- To build right away: **Actions → Build and release WordCraft Android APK → Run workflow**. You can enter an upstream
  branch or tag, or tick *force* to rebuild a version that already has a release.
- To stay on one upstream version, add a repository variable named `UPSTREAM_REF` (Settings → Secrets and variables →
  Actions → Variables) set to a tag or commit.

The Android layer lives in `android-layer/wordcraft-android-layer.tar.gz`. It adds an Android app crate
(`apps/wordcraft-android`), a patched `winit` for hardware keyboard and mouse input, and an Android manifest. It does not
change WordCraft's own code. If upstream changes in a way that breaks the layer, the build stops and the log says why.

## Signing

Builds are signed with a key the workflow creates and caches. For a permanent key, add the secrets
`ANDROID_KEYSTORE_B64` (a PKCS12 keystore, base64-encoded, with alias `wordcraft`) and `ANDROID_KEYSTORE_PASSWORD`.
APKs signed with different keys can't update each other.

## Credits and licence

- WordCraft is by the [ArtCraft](https://getartcraft.com/) team, under MIT or Apache-2.0.
- The Android approach follows the community PhotoCraft port,
  [SnowOwlLeader/photocraft-for-android](https://github.com/SnowOwlLeader/photocraft-for-android).
- `vendor/winit` is [winit](https://crates.io/crates/winit) 0.30.13 with a small local input patch (Apache-2.0).

This project is not affiliated with or supported by the WordCraft or ArtCraft team.
