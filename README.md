# It's just tmux!

This is a little PM on how I set it up last, should I forget.

## Software

For maximum productivity and ergonomics I'd like to have the following available:

### On Windows

* [The New Terminal](https://github.com/microsoft/terminal) to get some pretty things available.
* [Ubuntu on WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10) but any Linux should do.
* [Nerd Fonts](https://github.com/ryanoasis/nerd-fonts) so we get some extra nice symbols available.

### On WSL

* [Fish](https://fishshell.com/) (With the [Oh My Fish](https://github.com/oh-my-fish/oh-my-fish) extensions.)
* [tmux](https://github.com/tmux/tmux/wiki)
* [Nix](https://nixos.org/nix/)

## Installation

The installations on the Windows side is left as an exercise for the reader.

### Setting up the WSL base

Before opening WSL start the New Terminal and open settings. Find all `"fontFace"` entries and set to something you like from the [Nerd Fonts](https://github.com/ryanoasis/nerd-fonts) (or whatever fat unicode font you have and or like ...).

While you are at it find the profile for the installed Linux distro and change the `"commandLine"` to `"wsl.exe ~"`. This makes that profile short cut open the default installed distro and go to the users home.

Now enter WSL and let the installation finish. When you are given the prompt again, start by updating the base system, install the most recent version of the Fish shell with Oh My Fish and finally set it as the default shell.

```bash
sudo apt-add-repository ppa:fish-shell/release-3
sudo apt update
sudo apt upgrade
sudo apt install fish
curl -L https://get.oh-my.fish > install
fish install --path=~/.local/share/omf --config=~/.config/omf
chsh -s (which fish)
```

### Nix on WSL

Following the steps from Nathan Binjes [Nix on Microsoft Windows 10](https://nathan.gs/2019/04/12/nix-on-windows/) we create the file `/etc/nix/nix.conf` with the content:

```bash
sandbox = false
use-sqlite-wal = false
```

*Note to self: revisit after WSL 2, and change as necessary.*

Now get Nix installed by running:

```fish
curl https://nixos.org/nix/install | sh
omf install foreign-env
```

To get the Nix-related paths working (so that we can run `wsl.exe -e <nix-installed-package>`) copy the content of [Lilyballs](https://github.com/lilyball) excellent [Nix env configuration script](https://github.com/lilyball/nix-env.fish/blob/master/conf.d/nix-env.fish) to `~/.config/fish/conf.d/before.omf.fish`.

Now try to update the Nix channels and install tmux!

```fish
nix-channel --update
nix-env -i tmux
omf install tmux-zen
```

Exit and open again. We should now be in fish inside tmux with all Nix tools available!

## Wrap up

With the basic stuff in place we can now install Spacemacs, or some other editor if you prefer. Git is also a reasonable tool to have available at the top level and possibly some other things. But most stuff could (and should!) be put into [Nix Shell Things](NIXSHELLTHINGS.md)!

```fish
nix-env -i emacs
nix-env -i git
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
```

## And that my friend,

is the end.
