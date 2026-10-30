# flutter-ohos-engine-maven

Static Maven repository for the Android engine artifacts built from
`AAswordman/flutter-ohos` (Operit.9 line). It exists so builds can use the
patched Android engine (partial repaint, Vulkan/GLES) without
`--local-engine`.

## Artifacts

| Coordinate | Contents |
|---|---|
| `io.flutter:arm64_v8a_release:1.0.0-42d3d75a-operit.9.2` | `lib/arm64-v8a/libflutter.so` |
| `io.flutter:flutter_embedding_release:1.0.0-42d3d75a-operit.9.2` | Java embedding classes (unchanged from upstream) |

## Build provenance

- Source: `AAswordman/flutter-ohos`, `release/operit.9` / `46f42cae08a`
  (Operit.8 plus the partial-repaint engine commits).
- Configured with `--prebuilt-dart-sdk`, so the engine embeds the same Dart VM
  revision (`78da37fed6bf1489361a312568249f3f`) as the SDK's stock
  `gen_snapshot`. Only `libflutter.so` needs to be replaced.
- Target: Android arm64, runtime mode release, `engine.version` pin
  `42d3d75a56efe1a2e9902f52dc8006099c45d937`.

## Usage

Add this repository before the Flutter engine repository and force the
`io.flutter` version:

```kotlin
allprojects {
    repositories {
        maven { url = uri("https://h4rm00n.github.io/flutter-ohos-engine-maven") }
    }
    configurations.all {
        resolutionStrategy.eachDependency {
            if (requested.group == "io.flutter") {
                useVersion("1.0.0-42d3d75a-operit.9.2")
            }
        }
    }
}
```

The repository is a plain static tree (`gh-pages`); no authentication is
required.

## Build environment

Use JDK 17 for the Android build (the same version the project CI uses).
JDK 21 rejects type-use `@NotNull` annotations in some Flutter plugin
sources, which are only recompiled when the Gradle configuration changes.

## License

The Flutter engine is distributed under the BSD 3-Clause license. See
`LICENSE` in this repository.
