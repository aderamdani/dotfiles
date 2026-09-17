# After Install macOS

## 1. Install Tooling

### Xcode Command Line Tools

```bash
xcode-select --install
```

### Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### iTerm2

```bash
brew install --cask iterm2
```

### Opencode

```bash
brew install anomalyco/tap/opencode
```

API Key Opencode: isi sendiri di file ini setelah clone (JANGAN commit API key ke repo publik).

### Vorssaint

```bash
brew install --cask vorssaint
```

## 2. Optimasi Animasi & Kecepatan

### 2a. Dock Animation

| Key                         | Default | Recommended | Effect                |
| --------------------------- | ------- | ----------- | --------------------- |
| `autohide-delay`            | 0.5     | 0           | Dock muncul instan    |
| `autohide-time-modifier`    | 0.5     | 0.15        | Slide cepat           |
| `expose-animation-duration` | 0.25    | 0.1         | Mission Control cepat |
| `launchanim`                | true    | false       | Tanpa bounce          |
| `mineffect`                 | genie   | scale       | Minimize cepat        |
| `springboard-show-duration` | 0.25    | 0.1         | Launchpad cepat       |

```bash
defaults write com.apple.dock autohide-delay -float 0                   # tunda 0 = instan
defaults write com.apple.dock autohide-time-modifier -float 0.15        # slide 0.15s
defaults write com.apple.dock expose-animation-duration -float 0.1      # Mission Control 0.1s
defaults write com.apple.dock launchanim -bool false                     # matiin bounce
defaults write com.apple.dock mineffect -string scale                    # scale vs genie
defaults write com.apple.dock springboard-show-duration -float 0.1      # Launchpad muncul 0.1s
defaults write com.apple.dock springboard-hide-duration -float 0.1      # Launchpad hilang 0.1s
```

**minimize effect options**: `scale` (cepat), `genie` (bawaan, gelembung), `suck` (hisap)

### 2b. Window & UI Animation

| Key                                  | Default | Recommended | Effect               |
| ------------------------------------ | ------- | ----------- | -------------------- |
| `NSAutomaticWindowAnimationsEnabled` | true    | false       | Jendela instant      |
| `NSWindowResizeTime`                 | 0.2     | 0.001       | Resize instan        |
| `com.apple.sound.uiaudio.enabled`    | 1       | 0           | Suara UI mati        |
| `DisableAllAnimations` (Finder)      | false   | true        | Finder tanpa animasi |

```bash
defaults write NSGlobalDomain NSAutomaticWindowAnimationsEnabled -bool false   # matiin animasi jendela
defaults write NSGlobalDomain NSWindowResizeTime -float 0.001                  # resize instan
defaults write NSGlobalDomain com.apple.sound.uiaudio.enabled -int 0           # suara UI mati
defaults write com.apple.finder DisableAllAnimations -bool true                # Finder tanpa animasi
```

### 2c. Keyboard Speed

| Key                | Default        | Recommended | Effect       |
| ------------------ | -------------- | ----------- | ------------ |
| `KeyRepeat`        | 6 (~30 char/s) | 2           | Ketik cepat  |
| `InitialKeyRepeat` | 68 (~680ms)    | 15          | Repeat cepat |

```bash
defaults write NSGlobalDomain KeyRepeat -int 2            # repeat cepat
defaults write NSGlobalDomain InitialKeyRepeat -int 15    # delay 150ms
```

### 2d. Apply Semua Sekaligus

```bash
killall Dock && killall Finder
```

**Catatan**: `killall Dock` wajib agar Dock restart dengan setting baru.

### 2e. Reset ke Default

```bash
# Dock
defaults delete com.apple.dock autohide-time-modifier
defaults delete com.apple.dock autohide-delay
defaults delete com.apple.dock expose-animation-duration
defaults delete com.apple.dock launchanim
defaults delete com.apple.dock mineffect
defaults delete com.apple.dock springboard-show-duration
defaults delete com.apple.dock springboard-hide-duration

# Window & UI
defaults delete NSGlobalDomain NSAutomaticWindowAnimationsEnabled
defaults delete NSGlobalDomain NSWindowResizeTime
defaults delete NSGlobalDomain com.apple.sound.uiaudio.enabled
defaults delete com.apple.finder DisableAllAnimations

# Keyboard
defaults delete NSGlobalDomain KeyRepeat
defaults delete NSGlobalDomain InitialKeyRepeat

killall Dock && killall Finder
```

Atau **System Settings → Accessibility → Display → Reduce Motion** (toggle on/off).

### 2f. Check Current Values

```bash
defaults read com.apple.dock | grep -E "autohide|mineffect|launchanim|springboard"
defaults read NSGlobalDomain | grep -E "KeyRepeat|NSAutomatic|NSWindowResize"
```

### 2g. Troubleshooting

- Perubahan tidak muncul? `killall Dock && killall Finder`, atau logout/login.
- Dock hilang setelah killall? Tunggu 1-2 detik, atau: `open /System/Library/CoreServices/Dock.app`
- Keyboard repeat terlalu cepat? `defaults delete NSGlobalDomain KeyRepeat && defaults delete NSGlobalDomain InitialKeyRepeat`
- Kembali ke Genie minimize? `defaults write com.apple.dock mineffect -string genie && killall Dock`

## 3. Dock Setup

```bash
# Ukuran & auto-hide
defaults write com.apple.dock tilesize -int 40
defaults write com.apple.dock autohide -bool true

# Minimize windows masuk ke icon aplikasi
defaults write com.apple.dock minimize-to-application -bool true

# Reset isi Dock: sisa Finder + folder Applications saja
defaults write com.apple.dock persistent-apps -array
defaults write com.apple.dock persistent-others -array "<dict><key>GUID</key><string>{$(uuidgen)}</string><key>tile-data</key><dict><key>file-data</key><dict><key>_CFURLString</key><string>file:///Applications</string><key>_CFURLStringType</key><integer>15</integer></dict><key>file-label</key><string>Applications</string><key>file-type</key><integer>2</integer></dict></dict>"

killall Dock
```

Pin Apps (Launchpad-nya Tahoe) ke Dock:

```bash
defaults write com.apple.dock persistent-apps -array-add '<dict><key>GUID</key><string>{'"$(uuidgen)"'}</string><key>tile-data</key><dict><key>file-data</key><dict><key>_CFURLString</key><string>file:///System/Applications/Apps.app/</string><key>_CFURLStringType</key><integer>15</integer></dict><key>file-label</key><string>Apps</string><key>file-type</key><integer>3</integer></dict></dict>' && killall Dock
```

## 4. Widgets

```bash
defaults write com.apple.WindowManager StandardHideWidgets -bool true      # hide widget desktop
defaults write com.apple.WindowManager StageManagerHideWidgets -bool true  # hide widget Stage Manager
```

Widget di Notification Center: hapus manual via "Edit Widgets" di panel notifikasi.

Sembunyikan aplikasi iPhone/iPad dari daftar Apps (UI only):

- Buka **Apps** → tombol **(•••)** kanan atas → uncheck **Show iPhone Apps**, atau
- **System Settings → Spotlight → matikan iPhone Apps**.

## 5. Hot Corners

Target: kiri atas = Mission Control, kanan atas = Notification Center, kiri bawah = Apps, kanan bawah = Quick Note.

```bash
defaults write com.apple.dock wvous-tl-corner -int 2    # Mission Control
defaults write com.apple.dock wvous-tr-corner -int 12   # Notification Center
defaults write com.apple.dock wvous-bl-corner -int 11   # Apps / Launchpad
defaults write com.apple.dock wvous-br-corner -int 14   # Quick Note
defaults write com.apple.dock wvous-tl-mod -int 0
defaults write com.apple.dock wvous-tr-mod -int 0
defaults write com.apple.dock wvous-bl-mod -int 0
defaults write com.apple.dock wvous-br-mod -int 0
killall Dock
```

> CATATAN: di beberapa build Tahoe kode enum ini bisa bergeser. Kalau perilaku tidak cocok, set sekali manual via
> System Settings → Desktop & Dock → Hot Corners, lalu cek nilainya: `defaults read com.apple.dock | grep wvous`.
> Pastikan dialog Hot Corners sudah ditutup (⌘Q System Settings) sebelum mengetes, karena selama dialog terbuka
> value dari terminal tidak langsung berlaku.

## 6. Troubleshooting

- Perubahan tidak muncul? `killall Dock`, `killall Finder`, atau logout/login.
- Jangan klik **Done** pada dialog Hot Corners yang lama setelah setting dari terminal — itu menimpa ulang dengan draft lama.
- Beberapa perubahan butuh restart (bukan hanya killall Dock).
- **Reduce Motion** (System Settings → Accessibility → Display) override beberapa animasi.

## 7. Brew Applications

### Formula

```bash
brew install ada-url brotli bun c-ares ca-certificates fastfetch fmt gh \
  hdrhistogram_c icu4c@78 libffi libnghttp2 libnghttp3 libngtcp2 libuv \
  llhttp lz4 merve mpdecimal nbytes node oh-my-posh opencode openssl@3 \
  payload-dumper-go pcre2 python@3.12 readline ripgrep rtk ruff \
  simdjson simdutf sqlite tailscale ty uv uvwasi vercel xz yyjson zstd
```

### Cask

```bash
brew install --cask android-platform-tools bettercapture free-download-manager \
  iina iterm2 opencode-desktop terax vorssaint
```

## 8. Opencode Configuration

### Directory Structure

```bash
mkdir -p ~/.config/opencode/{agents,commands,plugins,skills}
```

### opencode.jsonc

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "shell": "zsh",
  "plugin": [
    "./plugins/rtk.ts",
    "token-optimizer-opencode",
    "./plugins/caveman/plugin.js",
    "opencode-dcp",
    "envsitter-guard",
    "opencode-worktree",
    "opencode-command-hooks",
  ],
  "lsp": {
    "ty": {
      "command": ["ty", "server"],
      "extensions": [".py", ".pyi"],
    },
    "ruff": {
      "command": ["ruff", "server"],
      "extensions": [".py", ".pyi"],
    },
    "typescript": {
      "command": ["typescript-language-server", "--stdio"],
      "extensions": [
        ".ts",
        ".tsx",
        ".js",
        ".jsx",
        ".mjs",
        ".cjs",
        ".mts",
        ".cts",
      ],
    },
  },
  "formatter": {
    "prettier-global": {
      "command": ["prettier", "--write", "$FILE"],
      "extensions": [
        ".js",
        ".jsx",
        ".ts",
        ".tsx",
        ".mjs",
        ".cjs",
        ".json",
        ".md",
        ".yaml",
        ".yml",
        ".html",
        ".css",
      ],
    },
  },
  "permission": {
    "grep": "allow",
    "glob": "allow",
    "bash": {
      "uv *": "allow",
      "uvx *": "allow",
      "ruff *": "allow",
      "ty *": "allow",
      "python3.12 *": "allow",
      "python3 *": "allow",
      "python *": "allow",
      "py *": "allow",
      "rg *": "allow",
      "node *": "allow",
      "npm *": "allow",
      "npx *": "allow",
      "prettier *": "allow",
      "tsc *": "allow",
      "typescript-language-server *": "allow",
      "gh *": "allow",
      "vercel *": "allow",
      "tailscale *": "allow",
      "writer": "allow",
      "writer *": "allow",
      "rtk": "allow",
      "rtk *": "allow",
    },
    "skill": {
      "*": "ask",
      "caveman": "allow",
      "caveman-*": "allow",
      "cavecrew": "allow",
    },
  },
}
```
