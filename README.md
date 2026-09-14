# 💻🎁 New Computer Checklist 🎁💻

A checklist for setting up a new or freshly reinstalled Mac. When it is done,
it should feel like a super-fast, brand-new machine!

Imagine your MacBook was stolen tomorrow—or you started a new job with a
machine fresh off the press. Could you recover your favorite settings without
having to remember everything from scratch?

The order is intentional: secure the Mac first, install the base tools,
restore personal configuration, and then tune preferences. Skip anything
that does not apply.

## 🔑 1. Before starting

Have these ready:

- [ ] Wi-Fi password
- [ ] Apple ID and a trusted device for two-factor authentication
- [ ] Password manager credentials and recovery information
- [ ] Backup or cloud-storage credentials
- [ ] Work account credentials, if applicable

Do not copy a private SSH key from this repository or another computer.
Generate a new key for each Mac so it can be revoked independently.

## 🔒 2. Secure and update macOS

- [ ] Install all updates in **System Settings → General → Software Update**
- [ ] Enable **FileVault** in **Privacy & Security → FileVault**
- [ ] Enable the firewall in **Network → Firewall**
- [ ] Require a password immediately after the display turns off
- [ ] Confirm **Find My Mac** is enabled
- [ ] Decide on a backup strategy and verify that it runs
- [ ] Leave **Guest User** disabled
- [ ] Review apps under **Privacy & Security** before granting Accessibility,
      Screen Recording, Full Disk Access, or Automation permissions

## 🐚 3. Install command-line tools

Install Apple's command-line tools:

```sh
xcode-select --install
```

Install [Homebrew](https://brew.sh/) with its current official installer:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow Homebrew's printed instructions to add it to the shell, then install
the essentials:

```sh
brew install bat gh git
brew install --cask cursor google-chrome iterm2 keycastr maccy
```

Useful tools:

- `bat` — `cat` with syntax highlighting
- `gh` — GitHub CLI
- [Maccy](https://maccy.app/) — clipboard manager
- [KeyCastr](https://github.com/keycastr/keycastr) — display keystrokes in recordings

## 🛠️ 4. Restore the development environment

### 🗂️ Git and dotfiles

- [ ] Clone the [personal dotfiles](https://github.com/grant/dotfiles)
- [ ] Follow that repository's setup instructions
- [ ] Configure Git:

```sh
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR EMAIL ADDRESS"
git config --global pull.ff only
git config --global push.default current
git config --global push.autoSetupRemote true
```

### 🔑 SSH

Follow [GitHub's current SSH instructions][github-ssh]. The short version for
a new Mac is:

```sh
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Add this host entry to `~/.ssh/config` so macOS reloads the key after a
restart:

```sshconfig
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Then load the key, copy its public half, add it in GitHub, and test it:

```sh
chmod 600 ~/.ssh/config
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
pbcopy < ~/.ssh/id_ed25519.pub
ssh -T git@github.com
```

### ⬛ Terminal

- [ ] Install [iTerm2](https://iterm2.com/)
- [ ] Import the [iTerm2 profile](https://github.com/grant/iterm-profile) and
      make it the default
- [ ] Install [Powerlevel10k](https://github.com/romkatv/powerlevel10k) and
      replace the dotfiles' `agnoster` theme:

```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \
  "${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k"
sed -i '' \
  's|^ZSH_THEME=.*|ZSH_THEME="powerlevel10k/powerlevel10k"|' ~/.zshrc
exec zsh
```

Run `p10k configure` if the configuration wizard does not start automatically.

- [ ] Set scrollback to 50,000 lines
- [ ] Enable copy-to-pasteboard on selection
- [ ] Use a vertical, blinking cursor
- [ ] Disable saving command and copy/paste history to disk

### 💻 Programming languages

Install only the runtimes needed for current projects. macOS does not provide
supported Python or Ruby development runtimes.

- [Node.js](https://nodejs.org/en/download): `brew install node`
- [Go](https://go.dev/dl/): `brew install go`
- [Java](https://adoptium.net/): `brew install --cask temurin`
- [.NET](https://dotnet.microsoft.com/en-us/download):
  `brew install --cask dotnet-sdk`
- Python: `brew install python`
- Ruby: `brew install ruby`
- PHP: `brew install php`

### 📝 [Cursor](https://cursor.com/download)

- [ ] Sign in to restore account-backed Cursor user rules
- [ ] Copy the relevant values from
      [`cursor-settings.json`](cursor-settings.json) into the user settings
- [ ] Install [Fira Code](https://github.com/tonsky/FiraCode):

```sh
brew install --cask font-fira-code
```

## 🖥️ 5. Install applications

### 📦 Everyday

- [Google Chrome](https://www.google.com/chrome/) — browser
- [Moom](https://manytricks.com/moom/) — window management
- [Screen Studio](https://www.screen.studio/) — screen recording
- [GIPHY Capture](https://giphy.com/apps/giphycapture) — quick GIF capture

For frequently used web apps such as Google Chat, use Chrome's
**Cast, save, and share → Install page as app** command. This replaces the
unmaintained Nativefier workflow.

### 🌐 Chrome

- [ ] Sign in and enable profile sync
- [ ] Set Chrome as the default browser
- [ ] Review synced extensions and remove anything no longer used
- [ ] Set Gmail as the default handler for `mailto:` links

Extensions worth restoring:

- Checker Plus for Gmail
- Checker Plus for Google Calendar
- GoFullPage
- Hacker News Collapsible Comments
- JSON Formatter
- Refined GitHub
- Video Speed Controller
- WhatFont

Prefer the browser's built-in password manager or the chosen standalone
password manager; do not maintain both.

## ⚙️ 6. Configure System Settings

Apple moves settings between releases. Search within System Settings when a
path has changed.

### 🍎 Appearance and desktop

- Appearance: Dark
- Accent color: Multicolor
- Sidebar icon size: Small
- Show scroll bars: Automatically
- Prefer tabs in full screen
- Disable Handoff
- Screen saver: start after one hour and show the clock
- Bottom-left Hot Corner: put display to sleep

### 🚢 Dock and menu bar

- Dock size: Smallest
- Enable magnification
- Position: Right
- Minimize using the Scale effect
- Disable launch animations
- Automatically hide the Dock
- Show indicators for open apps
- Keep only Finder, Chrome, iTerm2, and Cursor in the Dock
- Show day of week and date in the menu bar
- Show volume, battery, and Wi-Fi in the menu bar

### 📺 Displays and battery

- Automatically adjust brightness
- Enable True Tone
- Use ProMotion where available
- Turn the display off after 15 minutes on battery
- Enable optimized battery charging
- Slightly dim the display on battery

### ⌨️ Keyboard, mouse, and trackpad

- Key Repeat: second-fastest
- Delay Until Repeat: third tick from the left
- Use F1, F2, and so on as standard function keys
- Mouse tracking speed: fastest
- Trackpad tracking speed: second-fastest
- Enable secondary click, tap to click, and Force Click
- Enable the standard scroll, zoom, Spaces, Mission Control, and Show Desktop gestures

To set mouse tracking beyond the UI maximum:

```sh
defaults write -g com.apple.mouse.scaling -float 5
```

Log out and back in for the setting to take effect.

### 📂 Finder

- Default folder view: List
- Show Applications, Desktop, Documents, Downloads, external disks, and
  optical media in the sidebar
- Hide AirDrop, Movies, Music, Pictures, hard disks, and recent tags from the sidebar
- Show all filename extensions
- Search the current folder by default

### ☁️ Accounts, sharing, and privacy

- Keep only needed iCloud services enabled; keep Keychain and Find My Mac enabled
- Add Google Workspace accounts directly to the apps that need them
- Disable unused Sharing services
- Keep Bluetooth off when it is not needed
- Allow notifications as banners and show them in Notification Center
- Review Login Items and Extensions; remove anything unused

### 💬 Accessibility

- Enable keyboard shortcuts for zoom
- Zoom style: Full screen
- Keep VoiceOver, Switch Control, and Dictation disabled unless needed
- Enable “Shake mouse pointer to locate”

## 🧰 7. Apply macOS defaults

Run only the defaults still desired:

```sh
# Speed up Mission Control animations.
defaults write com.apple.dock expose-animation-duration -float 0.1

# Show hidden files in Finder.
defaults write com.apple.finder AppleShowAllFiles -bool true

# Avoid .DS_Store files on network volumes.
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true

# Hide Terminal's “Last login” message.
touch ~/.hushlogin

killall Finder
killall Dock
```

## ✅ 8. Final checks

- [ ] Restart the Mac
- [ ] Confirm FileVault encryption is progressing or complete
- [ ] Confirm the backup completed successfully
- [ ] Test GitHub SSH access with `ssh -T git@github.com`
- [ ] Open a project and verify its toolchain
- [ ] Check microphone, camera, screen sharing, and external displays
- [ ] Remove unused installers and Login Items

## 🖥️ Hardware baseline

- Apple Silicon MacBook Pro
- 16 GB or more RAM
- SSD storage sized for active projects

[github-ssh]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
