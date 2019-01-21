# Sommelier + Ubuntu + ChromeOS

## Prerequisites
Crouton downloaded into `~/Downloads/`

## Run Crouton
```bash
cd ~/Downloads/
sudo sh crouton -t cli-extra -r artful
```

## Log in
```bash
sudo enter-chroot
```

## Setup Environment Variables and alias (might want to add to ~/.bashrc)
```bash
export GDK_BACKEND=wayland
export CLUTTER_BACKEND=wayland
export XDG_RUNTIME_DIR='/var/run/chrome'
export WAYLAND_DISPLAY=wayland-0
export DISPLAY=:0
alias sommelier="${HOME}/Downloads/sommelier/bin/sommelier -X --x-display=:0"
```

## Install Dependencies
```bash
sudo apt-get install -y pkg-config build-essential git make xwayland libwayland-dev libgbm-dev gcc libx11-xcb-dev libsystemd-dev libxcb-composite0-dev libxkbcommon-dev libxrender-dev libxtst-dev libpixman-1-dev
```

## Get and run Sommelier Service
```bash
cd ~/Downloads/
git clone https://chromium.googlesource.com/chromiumos/containers/sommelier.git
cd sommelier
git reset --hard 1382ce084cc40790340d672e8b62ec47733cb860
sed -i -e 's,\$(PREFIX)/bin,\$(BINDIR),g' -e 's,\$(BINDIR)/Xwayland,`which Xwayland`,g' Makefile
make PREFIX=~/Downloads/sommelier/bin BINDIR=\$\(PREFIX\) SYSCONFDIR=~/Downloads/sommelier/bin/etc
make install PREFIX=~/Downloads/sommelier/bin BINDIR=\$\(PREFIX\) SYSCONFDIR=~/Downloads/sommelier/bin/etc
./bin/sommelier -X --x-display=:0 --no-exit-with-child /bin/sh -c "./bin/etc/sommelierrc" &

```

## Done!
You're ready to go! Open an app in another terminal.

![Intellij Running](intellij.png)
