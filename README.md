# Stefano Coniglio's Emacs configuration

My Emacs init file, `init.el`. It is the live file, not a copy: `~/.emacs.d/init.el` is a
symlink to it.

```
git clone https://github.com/stefanoconiglio/emacs-config ~/repos/emacs-config
ln -s ~/repos/emacs-config/init.el ~/.emacs.d/init.el
```

Written for Emacs 31 on Omarchy (Arch Linux), started mostly as a daemon (`emacs.service`) and
opened through "Emacs (Client)". The daemon reads the file once, when it starts: after a change,
restart it (`systemctl --user restart emacs`) once everything is saved.

## What it needs

- **Packages** from MELPA, MELPA Stable and GNU ELPA, installed on first start (`use-package :ensure`).
  Two are not on an archive and come from their repositories through `use-package :vc`:
  ampl-mode (github.com/ampl/ampl-mode) and claude-code-ide
  (github.com/manzaltu/claude-code-ide.el).
- **Three checkouts**, loaded in graphical Emacs and in the daemon:
  - [pdf-view-scroll-sync](https://github.com/stefanoconiglio/emacs-pdf-view-scroll-sync) at
    `~/repos/emacs-pdf-view-scroll-sync` (the PDF-VIEW-SCROLL-SYNC block): LaTeX source and PDF side by side,
    kept in step both ways;
  - [emacs-omarchy-theme](https://github.com/stefanoconiglio/emacs-omarchy-theme) at
    `~/repos/emacs-omarchy-theme` (same block): Emacs and its PDFs follow the Omarchy theme;
  - [texpresso-view](https://github.com/stefanoconiglio/emacs-texpresso-view) at
    `~/repos/emacs-texpresso-view` (the TEXPRESSO-VIEW block): `M-x texpresso-view`, the page re-typeset by
    [TeXpresso](https://github.com/let-def/texpresso) as you type, beside the source; TeXpresso built in
    `~/Installs/texpresso` with that package's patches.
- **Some absolute paths** remain (the conda environments in `~/miniconda3/envs`, for instance).

## Terminal frames

In `emacsclient -t` or `emacs -nw` the mouse works through `xterm-mouse-mode`, and a notch of the wheel scrolls
`my-tty-wheel-lines` lines (3; a terminal sends one wheel event a notch, which scrolled a single line). Graphical
frames keep `mouse-wheel-scroll-amount` (a touchpad sends many events), and Shift and Control with the wheel do
what they did in both. To change the step: `(setq my-tty-wheel-lines 5)`; to drop it:
`(advice-remove 'mwheel-scroll #'my-tty-wheel-scroll)`.

## Claude Code in Emacs

claude-code-ide runs Claude Code in a terminal inside Emacs and tells it which file is open and
which region is selected. `M-x claude-code-ide` starts it for the current project; `C-c C-'`
opens its menu in graphical frames (a terminal cannot send `C-'` to `emacs -nw`).

- **Which `claude`:** the mise-installed binary,
  `~/.local/share/mise/installs/claude/latest/claude`, the one `claude` runs in a terminal; plain
  `claude` only when that is missing. The daemon started at boot has PATH
  `/usr/local/bin:/usr/bin`, without claude, and a later PATH finds Omarchy's `mise use` wrapper
  `~/.local/bin/claude` first.
- **Its shell commands** see the daemon's PATH, `/usr/local/bin:/usr/bin` (Claude Code's bash
  does not read `~/.bashrc`, which stops when not interactive): `init.el` adds `~/.local/bin`
  and mise's shims (`~/.local/share/mise/shims`) after the rest, so your own commands and what
  mise installs are found and `/usr/bin/emacs` still comes before the wrapper in
  `~/.local/bin`. An Emacs started from a terminal, whose PATH has both, is left as it is.
- **Its edits** show at once: buffers follow their files when they change on disk
  (`global-auto-revert-mode`, checked every second).
- **`/voice`** works in its terminal: hold space to talk ("listening…").
- **Terminal:** ghostel (MELPA), which draws Claude Code's screen with fewer glitches than vterm.
  Its native module is not in the package: Emacs offers to download it on first use, or run
  `M-x ghostel-download-module`. Terminal buffers (ghostel, vterm) have no line numbers.
- **Window:** below the left pane when the frame has panes side by side (TeX source | PDF), else
  beside the selected window (`my-claude-code-ide-display`).

## Related

- [emacs-tui-default](https://github.com/stefanoconiglio/emacs-tui-default): terminal Emacs
  (`emacs -nw`) as Omarchy's default editor.
- Until 2026-10-03 this file was `home/.emacs.d/init.el` in omarchy-customizations (now omarchy-config), the private
  repository of the rest of the machine's customizations, which keeps its earlier history.
- Until 2026-10-24 this repository was emacs-customizations.

## License

GPL-3.0-or-later. See [LICENSE](LICENSE).
