# macOS Dock & Animation Speed — Complete Guide

Semua command untuk mempercepat animasi Dock, jendela, dan UI macOS.

---

## Table of Contents

1. [Dock Animation Settings](#1-dock-animation-settings)
2. [Window & UI Animation Settings](#2-window--ui-animation-settings)
3. [Keyboard Speed](#3-keyboard-speed)
4. [Apply All At Once](#4-apply-all-at-once)
5. [Reset to Default](#5-reset-to-default)
6. [Reference Table](#6-reference-table)
7. [Troubleshooting](#7-troubleshooting)

---

## 1. Dock Animation Settings

### Auto-hide Delay

```bash
defaults write com.apple.dock autohide-delay -float 0
```

- **Fungsi**: Waktu tunda sebelum Dock muncul saat cursor ke bawah layar
- **Default**: `0.5` detik
- **Nilai**: `0` = instan, `0.5` = bawaan, `1` = lambat
- **Tips**: `0` untuk respons tercepat

### Auto-hide Time Modifier

```bash
defaults write com.apple.dock autohide-time-modifier -float 0.15
```

- **Fungsi**: Durasi animasi slide-in/out Dock
- **Default**: `0.5` detik
- **Nilai**: `0.1` = sangat cepat, `0.15` = cepat, `0.5` = bawaan
- **Tips**: `0.15` sweet spot — cepat tapi masih smooth

### Expose Animation Duration

```bash
defaults write com.apple.dock expose-animation-duration -float 0.1
```

- **Fungsi**: Kecepatan animasi Mission Control & App Exposé
- **Default**: `0.25` detik
- **Nilai**: `0.1` = cepat, `0.25` = bawaan, `0.5` = lambat
- **Tips**: `0.1` untuk transisi instant

### Launch Animation

```bash
defaults write com.apple.dock launchanim -bool false
```

- **Fungsi**: Animasi bounce saat aplikasi dibuka dari Dock
- **Default**: `true` (bounce aktif)
- **Nilai**: `false` = mati, `true` = bounce
- **Tips**: `false` untuk langsung buka tanpa animasi

### Minimize Effect

```bash
defaults write com.apple.dock mineffect -string scale
```

- **Fungsi**: Efek saat minimize window
- **Default**: `genie`
- **Nilai**: `scale` = cepat, `genie` = efek gelembung (bawaan), `suck` = efek hisap
- **Tips**: `scale` 30-50% lebih cepat dari `genie`

### Springboard Show/Hide Duration

```bash
defaults write com.apple.dock springboard-show-duration -float 0.1
defaults write com.apple.dock springboard-hide-duration -float 0.1
```

- **Fungsi**: Durasi animasi Launchpad muncul/hilang
- **Default**: `0.25` detik
- **Nilai**: `0.1` = cepat, `0.25` = bawaan
- **Tips**: Bisa diabaikan kalau jarang pakai Launchpad

---

## 2. Window & UI Animation Settings

### Disable Window Animations

```bash
defaults write NSGlobalDomain NSAutomaticWindowAnimationsEnabled -bool false
```

- **Fungsi**: Matikan semua animasi buka/tutup jendela
- **Default**: `true`
- **Nilai**: `false` = mati, `true` = aktif
- **Tips**: Paling impactful — jendela langsung muncul tanpa fade

### Window Resize Time

```bash
defaults write NSGlobalDomain NSWindowResizeTime -float 0.001
```

- **Fungsi**: Kecepatan animasi resize jendela
- **Default**: `0.2` detik
- **Nilai**: `0.001` = instan, `0.2` = bawaan
- **Tips**: `0.001` untuk resize tanpa delay

### Disable UI Sound

```bash
defaults write NSGlobalDomain com.apple.sound.uiaudio.enabled -int 0
```

- **Fungsi**: Matikan suara efek UI (click, alert, dsb)
- **Default**: `1` (aktif)
- **Nilai**: `0` = mati, `1` = aktif
- **Tips**: Opsional — suara bisa mengganggu workflow

### Disable Finder Animations

```bash
defaults write com.apple.finder DisableAllAnimations -bool true
```

- **Fungsi**: Matikan animasi di Finder (folder open/close)
- **Default**: `false`
- **Nilai**: `true` = mati, `false` = aktif
- **Tips**: Finder jadi lebih responsif

---

## 3. Keyboard Speed

### Key Repeat

```bash
defaults write NSGlobalDomain KeyRepeat -int 2
```

- **Fungsi**: Kecepatan karakter berulang saat tombol ditekan terus
- **Default**: `6` (~30 char/detik)
- **Nilai**: `1` = sangat cepat (~600 char/detik), `2` = cepat, `6` = bawaan
- **Tips**: `2` untuk power user

### Initial Key Repeat

```bash
defaults write NSGlobalDomain InitialKeyRepeat -int 15
```

- **Fungsi**: Waktu tunda sebelum key repeat dimulai
- **Default**: `68` (~680ms)
- **Nilai**: `15` = 150ms (instan), `68` = bawaan
- **Tips**: `15` untuk respons secepat mungkin

---

## 4. Apply All At Once

Copy-paste block ini sekali jalan:

```bash
# Dock
defaults write com.apple.dock autohide-time-modifier -float 0.15
defaults write com.apple.dock autohide-delay -float 0
defaults write com.apple.dock expose-animation-duration -float 0.1
defaults write com.apple.dock launchanim -bool false
defaults write com.apple.dock mineffect -string scale
defaults write com.apple.dock springboard-show-duration -float 0.1
defaults write com.apple.dock springboard-hide-duration -float 0.1

# Window & UI
defaults write NSGlobalDomain NSAutomaticWindowAnimationsEnabled -bool false
defaults write NSGlobalDomain NSWindowResizeTime -float 0.001
defaults write NSGlobalDomain com.apple.sound.uiaudio.enabled -int 0
defaults write com.apple.finder DisableAllAnimations -bool true

# Keyboard
defaults write NSGlobalDomain KeyRepeat -int 2
defaults write NSGlobalDomain InitialKeyRepeat -int 15

# Apply
killall Dock && killall Finder
```

**Catatan**: `killall Dock` wajib agar Dock restart dengan setting baru.

---

## 5. Reset to Default

Jika ingin kembali ke setting bawaan:

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

# Apply
killall Dock && killall Finder
```

Atau **System Settings → Accessibility → Display → Reduce Motion** (toggle on/off).

---

## 6. Reference Table

| Key                                  | Default | Recommended | Effect                |
| ------------------------------------ | ------- | ----------- | --------------------- |
| `autohide-delay`                     | 0.5     | 0           | Dock muncul instan    |
| `autohide-time-modifier`             | 0.5     | 0.15        | Slide cepat           |
| `expose-animation-duration`          | 0.25    | 0.1         | Mission Control cepat |
| `launchanim`                         | true    | false       | Tanpa bounce          |
| `mineffect`                          | genie   | scale       | Minimize cepat        |
| `springboard-show-duration`          | 0.25    | 0.1         | Launchpad cepat       |
| `NSAutomaticWindowAnimationsEnabled` | true    | false       | Jendela instant       |
| `NSWindowResizeTime`                 | 0.2     | 0.001       | Resize instan         |
| `KeyRepeat`                          | 6       | 2           | Ketik cepat           |
| `InitialKeyRepeat`                   | 68      | 15          | Repeat cepat          |

---

## 7. Troubleshooting

### Perubahan tidak muncul

```bash
killall Dock && killall Finder
```

Jika masih tidak jalan: **Logout → Login** atau restart Mac.

### Dock hilang setelah killall

Dock akan restart otomatis dalam 1-2 detik. Jika tidak, buka Terminal:

```bash
open /System/Library/CoreServices/Dock.app
```

### Keyboard repeat terlalu cepat

Reset ke default:

```bash
defaults delete NSGlobalDomain KeyRepeat
defaults delete NSGlobalDomain InitialKeyRepeat
killall Dock
```

### Ingin kembali ke Genie minimize

```bash
defaults write com.apple.dock mineffect -string genie
killall Dock
```

### Check current values

```bash
defaults read com.apple.dock | grep -E "autohide|mineffect|launchanim|springboard"
defaults read NSGlobalDomain | grep -E "KeyRepeat|NSAutomatic|NSWindowResize"
```

---

## Catatan Tambahan

- **Reduce Motion**: System Settings → Accessibility → Display → Reduce Motion. Toggle ini override beberapa animasi.
- **Battery Impact**: Animasi lebih cepat = sedikit hemat CPU, tapi perbedaan tidak signifikan.
- **Compatibility**: Semua command di atas work di macOS Ventura, Sonoma, dan Sequoia.
- **Restart**: Beberapa perubahan mungkin butuh restart (bukan hanya killall Dock).

---

_Last updated: September 2026_
