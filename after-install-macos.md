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
brew uninstall --cask vorssaint
```

## 2. Optimasi Animasi & Kecepatan

Jalankan semua, lalu `killall Dock && killall Finder`.

```bash
# Dock: animasi super cepat
defaults write com.apple.dock autohide-time-modifier -float 0.15
defaults write com.apple.dock autohide-delay -float 0
defaults write com.apple.dock expose-animation-duration -float 0.1
defaults write com.apple.dock launchanim -bool false
defaults write com.apple.dock mineffect -string scale        # minimize pakai Scale, bukan Genie
defaults write com.apple.dock springboard-show-duration -float 0.1
defaults write com.apple.dock springboard-hide-duration -float 0.1

# Jendela & UI: matikan animasi
defaults write NSGlobalDomain NSAutomaticWindowAnimationsEnabled -bool false
defaults write NSGlobalDomain NSWindowResizeTime -float 0.001
defaults write NSGlobalDomain com.apple.sound.uiaudio.enabled -int 0   # matiin suara UI
defaults write com.apple.finder DisableAllAnimations -bool true

# Keyboard lebih ngebut
defaults write NSGlobalDomain KeyRepeat -int 2
defaults write NSGlobalDomain InitialKeyRepeat -int 15
```

Manual (tidak bisa via terminal): **System Settings → Accessibility → Display → Reduce Motion = ON**.

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
