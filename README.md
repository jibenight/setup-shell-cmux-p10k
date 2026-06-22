# Setup zsh + Powerlevel10k (cmux / iTerm2) for macOS

Personal shell setup: **oh-my-zsh + Powerlevel10k** theme, plugins, a Nerd Font,
and per-terminal configuration. Works in any terminal — the prompt lives in
`~/.zshrc` + `~/.p10k.zsh`, independent of the terminal app.

> **Terminal used today: [cmux](https://cmux.com).** The iTerm2 instructions are
> kept below as legacy reference.

---

## 1. Shell base (terminal-agnostic)

### Install oh-my-zsh
https://ohmyz.sh — or clone manually:
```sh
git clone --depth=1 https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh
```

### Install Powerlevel10k + plugins (as oh-my-zsh custom)
```sh
ZC=~/.oh-my-zsh/custom
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git       $ZC/themes/powerlevel10k
git clone --depth=1 https://github.com/zsh-users/zsh-autosuggestions     $ZC/plugins/zsh-autosuggestions
git clone --depth=1 https://github.com/zsh-users/zsh-syntax-highlighting $ZC/plugins/zsh-syntax-highlighting
```

In `~/.zshrc`:
```sh
ZSH_THEME="powerlevel10k/powerlevel10k"
plugins=(zsh-autosuggestions zsh-syntax-highlighting git)
```

### Prompt config
Copy `custom-p10k/.p10K.zsh` from this repo to `~/.p10k.zsh`.
(Style: **rainbow / powerline**, tuned for higher contrast — bright host text,
vivid cyan dir background, pure-black/white foregrounds.)

To regenerate from scratch: `p10k configure`.

### Font (required for the prompt glyphs)
Any **Nerd Font** works. Current choice is **FiraCode Nerd Font** (ligatures);
**MesloLGS Nerd Font** is the alternative recommended by Powerlevel10k.
```sh
brew install --cask font-fira-code-nerd-font    # current
brew install --cask font-meslo-lg-nerd-font     # p10k's official recommendation
```

---

## 2. cmux configuration

cmux embeds the **Ghostty** terminal engine and reads its config from:

- `~/.config/ghostty/config`
- `~/Library/Application Support/com.mitchellh.ghostty/config`

Terminal rendering (background, font, contrast…) is set in the **Ghostty config**,
not in `cmux.json`. The cmux GUI (`Cmd+,`) only exposes behavior + app/sidebar appearance.

### Terminal look — `cmux/ghostty-config` → `~/.config/ghostty/config`
```ini
background       = #000000              # pure black (default theme was #282c34, grey)
font-family      = FiraCode Nerd Font Mono   # or: MesloLGS Nerd Font Mono (p10k's pick)
minimum-contrast = 1.1                  # force a minimum legible contrast (1 = off, ~21 = b/w)
font-thicken     = true                 # thicker glyphs — big readability win on Retina
```

### App / sidebar — `cmux/cmux.json` → `~/.config/cmux/cmux.json`
```json
{
  "sidebarAppearance": { "matchTerminalBackground": true }
}
```
Makes the sidebar use the (black) terminal background instead of its grey tint.

### Apply changes
```sh
cmux reload-config        # or press Cmd+Shift+,
```

---

## 3. iTerm2 (legacy)

Kept for reference — no longer used.

- Install iTerm2: https://iterm2.com
- Same oh-my-zsh / Powerlevel10k / plugins / `.p10k.zsh` as above.
- Set the iTerm2 font to **MesloLGS NF** (Preferences → Profiles → Text).

---

## 4. SSH key for GitHub

Generate a key (press Enter for no passphrase):
```sh
ssh-keygen -t ed25519 -C "your_email@example.com"   # or: ssh-keygen -t rsa
ls ~/.ssh
pbcopy < ~/.ssh/id_ed25519.pub                       # copy the PUBLIC key
```
Then add it on GitHub → Settings → SSH and GPG keys.
Docs: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh

---

## Files in this repo
```
custom-p10k/.p10K.zsh   # Powerlevel10k prompt config   → ~/.p10k.zsh
cmux/ghostty-config     # cmux terminal (Ghostty) config → ~/.config/ghostty/config
cmux/cmux.json          # cmux app/sidebar settings      → ~/.config/cmux/cmux.json
```
