# Quasar App (quasar-sharp)

A Quasar Project to find out how to include native modules like sharp into an Electron app.

It works when running in dev mode, but not packaged:
![Error](error.png)

I tried to follow the advices given on the sharp website (see links below) and tinkered with more hints on my original project. I just wanted to keep this demo relatively clean.

## Project Scaffolding

- `yarn create quasar`
- Accept all defaults (Quasar CLI with Vite)
- `quasar mode add electron`

### Additions/actions for sharp

- `yarn add sharp --ignore-engines` (`--ignore-engines` is recommended for yarn v1)
- Added sharp test call to `electron-main.js` -> Runs in dev mode, but not as packaged build.
- Added unpack options to `quasar.config.js`, both to packager and to builder sections.

### Infos on Adding sharp

- [sharp and Electron](https://sharp.pixelplumbing.com/install#electron). Where to put these build options? Into `quasar.config.js` under electron - packager?
- [asarUnpack](https://github.com/lovell/sharp/issues/3985). Does not work with packager?
- [Configuring Electron](https://quasar.dev/quasar-cli-vite/developing-electron-apps/configuring-electron)

### Questions

- Do I need to use builder instead of packager? `asarUnpack` seems not to be available in packager.
- Do I need yarn v3 (lovell: "_My current advice to anyone working on multi or cross platform Node.js apps is to use either yarn 3+ or pnpm to manage dependencies via the supportedArchitectures feature._")? Seems so.

### Progress

- Using yarn v4: Dev version still builds and runs.
- Using builder: Produces a packaged version which runs, but sharp is not included (`sharp@npm:0.33.4 must be built because it never has been before or the last one failed`). How can I build it for production?
- First `yarn install`in a new folder states

```
➤ YN0000: ┌ Link step
➤ YN0007: │ electron@npm:31.1.0 must be built because it never has been before or the last one failed
➤ YN0007: │ sharp@npm:0.33.4 must be built because it never has been before or the last one failed
➤ YN0007: │ esbuild@npm:0.14.51 must be built because it never has been before or the last one failed
➤ YN0007: │ esbuild@npm:0.14.54 must be built because it never has been before or the last one failed
➤ YN0000: └ Completed in 15s 390ms
```

- Subsequent `quasar build -m electron` states

```
➤ YN0000: ┌ Link step
➤ YN0007: │ sharp@npm:0.33.4 must be built because it never has been before or the last one failed
➤ YN0000: └ Completed in 2s 351ms
```

`esbuild` and `electron` do not have to be build again, but sharp does?

- Tried on Windows 10, same result as on macOS.
- Added @electron/rebuild and issued `./node_modules/.bin/electron-rebuild`, immediately results in `✔ Rebuild Complete`. Subsequent `quasar build -m electron` showed same results (sharp build took longer, but nothing else changed).

## General Info

### Start the app in development mode (hot-code reloading, error reporting, etc.)

```bash
quasar dev -m electron
```

### Build the app for production

```bash
quasar build -m electron
```

### Customize the configuration

See [Configuring quasar.config.js](https://v2.quasar.dev/quasar-cli-vite/quasar-config-js).
