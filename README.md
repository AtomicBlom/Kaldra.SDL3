# Kaldra.SDL3

[SDL 3](https://github.com/libsdl-org/SDL)'s native libraries as a NuGet package, `BinaryVibrance.Kaldra.SDL3`, and
nothing else: no bindings. Kaldra's desktop host (gamepads) and Sirrow (audio) both take it, so an app that uses both
loads one `SDL3.dll`.

- **What's in it:** `runtimes/win-x64/native/SDL3.dll` and `runtimes/win-arm64/native/SDL3.dll`, SDL's own release
  builds, each with SDL's licence beside it. SDL publishes Windows builds only. On other platforms the package carries
  nothing.
- **Versions follow SDL's:** package 3.4.20 is SDL 3.4.20. A fourth part (3.4.20.1) is a repackaging of the same SDL.
  A consumer names the lowest version it needs, so an app that takes two consumers gets the higher of the two.
- **Where it loads from:** a build without a runtime identifier puts each DLL under `runtimes/<rid>/native/` beside
  the app. A publish for one runtime puts that runtime's beside the executable.

## Building

```
dotnet pack Kaldra.SDL3 -c Release -o artifacts/packages
```

The build downloads SDL's release zips from GitHub, checks each against its pinned SHA-256, and fails if either
doesn't match. Nothing binary is kept in git. To move to a newer SDL, change `SdlVersion` and the two hashes in
`Kaldra.SDL3/Kaldra.SDL3.csproj`. To repackage the same SDL, pass `-p:PackageRevision=1`.

## Releasing

Push a tag named after the version, `v3.4.20`. CI packs the package and publishes it to nuget.org through trusted
publishing: it needs a policy for this repository and `pack.yml` on nuget.org, and the nuget.org user name as the
`NUGET_USER` secret. No API key is stored.

## Licence

SDL is under the [zlib licence](https://github.com/libsdl-org/SDL/blob/main/LICENSE.txt), and the package is
published under it. This repository's own build files are MIT (`LICENSE`).
