# Basic TypeScript templates starter for Embedded Systems

> [!IMPORTANT]
> This starter template is meant to be used with the Moddable SDK, which is a JS runtime for embedded systems.
> Learn how to setup an Embedded JavaScript project using [xs-dev](https://xs-dev.js.org/).

## Installation

For the sake of simplicity, we advise you to use mise-en-place to manage your project.

https://mise.jdx.dev/getting-started.html

This allows to install, package manager and other dependencies for the Moddable SDK and things easily. No more python issues, or node version mismatch.

Requirements:

- Package manager, `pnpm` recommended (`mise use -g pnpm` or https://pnpm.io/installation)
- Bundler, to transform all file in a single file and resolves modules. Using Bun builder in this repo, but could be esbuild or any other, but you'll need to adapt the configuration accordingly.
  `mise use -g bun` or https://bun.sh/
- Python, v3 is nice and easy to install with `mise use -g python`
- Moddable SDK, easy install with [xs-dev ](https://xs-dev.js.org/features/setup/)

You might see a warning

```
mise ERROR Config file ~/dev-workspace/OSS/xs-dev/mise.toml is not trusted.
Trust it with `mise trust`.
```

Therefore you can check the content of the file on your own and trust it as asked.

Once we have all, we can play around.

## Usage

Run `pnpm install` to install the dependencies.

The root file is `main.ts` and it's the entry point of the application. Then the bundler with crawl all the files and resolve the modules and dependencies, all of this in a single file `dist/main.js`.

Before injecting the code to the emulator or device we need to transpile the TypeScript files. This is done with the `pnpm build` command. This will generate the `dist` folder with the transpiled files.

- `pnpm build:dev` will transpile whenever a file change automatically using a watcher.
- `pnpm build:release` will transpile the files once and minify the output, optimizing the code for production.

Therefore you can just do the following

### In development

Open a shell and run the following commands:

```bash
pnpm build:dev
```

Then open another shell and run the following command whenever you want to send the code to the device/emulator:

```bash
xs-dev run
```

### In release

Open a shell and run the following commands:

```bash
pnpm build:release
```

Then open another shell and run the following command whenever you want to send the code to the device/emulator:

```bash
xs-dev build --mode production
```

For more details about building and running the code, please refer to the https://xs-dev.js.org/features/run/.
What this template does is only manage the TypeScript files and the bundling process. The rest is up to you.

## Additional information

To support what I call « injected libraries » we define `--packages external` to the bundler, this allows to not package the libraries that are already in the Moddable SDK. As current API are not available in `node_modules` we need to do that.

Bun treats any import which path do not start with `.`, `..` or `/` as package.
