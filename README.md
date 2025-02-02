# MODIFIED FORK
## Purpose
Hot-fix issues preventing usage of this library. Ad-hoc.

## Addressed Issues
- Any attempt to use the plugin fails due to JS errors
- The `download` api in the Guest JS still invokes the upload path in the Rust backend

![plugin-upload](banner.png)

Upload files from disk to a remote server over HTTP.

## Install

_This plugin requires a Rust version of at least **1.64**_

There are three general methods of installation that we can recommend.

1. Use crates.io and npm (easiest, and requires you to trust that our publishing pipeline worked)
2. Pull sources directly from Github using git tags / revision hashes (most secure)
3. Git submodule install this repo in your tauri project and then use file protocol to ingest the source (most secure, but inconvenient to use)

Install the Core plugin by adding the following to your `Cargo.toml` file:

`src-tauri/Cargo.toml`

```toml
[dependencies]
tauri-plugin-upload = { git = "https://github.com/tauri-apps/plugins-workspace", branch = "dev" }
```

You can install the JavaScript Guest bindings using your preferred JavaScript package manager:

> Note: Since most JavaScript package managers are unable to install packages from git monorepos we provide read-only mirrors of each plugin. This makes installation option 2 more ergonomic to use.

```sh
pnpm add https://github.com/tauri-apps/tauri-plugin-upload
# or
npm add https://github.com/tauri-apps/tauri-plugin-upload
# or
yarn add https://github.com/tauri-apps/tauri-plugin-upload
```

## Usage

First you need to register the core plugin with Tauri:

`src-tauri/src/main.rs`

```rust
fn main() {
    tauri::Builder::default()
        .plugin(tauri_plugin_upload::init())
        .run(tauri::generate_context!())
        .expect("error while running tauri application");
}
```

Afterwards all the plugin's APIs are available through the JavaScript guest bindings:

```javascript
import { upload } from 'tauri-plugin-upload-api'

upload(
    'https://example.com/file-upload'
    './path/to/my/file.txt'
    (progress, total) => console.log(`Downloaded ${progress} of ${total} bytes`) // a callback that will be called with the upload progress
    { 'ContentType': 'text/plain' } // optional headers to send with the request
)
```

## Contributing

PRs accepted. Please make sure to read the Contributing Guide before making a pull request.

## License

Code: (c) 2015 - Present - The Tauri Programme within The Commons Conservancy.

MIT or MIT/Apache 2.0 where applicable.
