# SoLoader Maven Central metadata reproducer

Minimal Android project for [meta/SoLoader#158](https://github.com/meta/SoLoader/issues/158).

| Branch | Default SoLoader version | Result on 2026-11-09 |
| --- | --- | --- |
| [repro](https://github.com/YuraBobeshko/soloader-maven-metadata-repro/tree/repro) | 0.10.4+ | Dependency resolution fails |
| [pinned](https://github.com/YuraBobeshko/soloader-maven-metadata-repro/tree/pinned) | 0.10.4 | APK builds successfully |

The branches differ by one character in the default dependency version.
[Compare the branches](https://github.com/YuraBobeshko/soloader-maven-metadata-repro/compare/repro...pinned).

## Requirements

- JDK 21, as used for the local verification.
- Android SDK Platform 36 and Build Tools 35.0.0.
- ANDROID_HOME set to the Android SDK directory.
- Network access to Maven Central, Google Maven, and Gradle distributions.

The wrapper uses Gradle 9.0.0. The project uses Android Gradle Plugin 8.12.0.
SoLoader is resolved from Maven Central.

## Reproduce

Run these commands in PowerShell.

```powershell
git clone https://github.com/YuraBobeshko/soloader-maven-metadata-repro.git
cd soloader-maven-metadata-repro
git switch repro
.\gradlew.bat --refresh-dependencies clean :app:assembleDebug
```

The build fails with this error.

```text
Could not find any version that matches com.facebook.soloader:soloader:0.10.4+.
Versions that do not match: 0.13.0
```

Switch to the pinned version and run the same build.

```powershell
git switch pinned
.\gradlew.bat --refresh-dependencies clean :app:assembleDebug
```

The build produces `app/build/outputs/apk/debug/app-debug.apk`.

On macOS or Linux, use `./gradlew` in place of `.\gradlew.bat`.
You can also select a version explicitly with `"-PsoloaderVersion=0.10.4+"`
or `"-PsoloaderVersion=0.10.4"` on either branch.

The results depend on the current [Maven Central metadata](https://repo.maven.apache.org/maven2/com/facebook/soloader/soloader/maven-metadata.xml).
The dynamic version can resolve again when the metadata includes the older releases.
