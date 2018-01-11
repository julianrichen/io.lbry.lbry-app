# io.lbry.lbry-app
A browser and wallet for LBRY, the decentralized, user-controlled content marketplace.

* Website: https://lbry.io/
* LBRY Repo: https://github.com/lbryio/lbry-app
* LBRY Issues: https://github.com/lbryio/lbry-app/issues
* Packaging Issues: https://github.com/flathub/io.lbry.lbry-app/issues

## Install
Are you a user? Install `flatpak` from your distribution's repository and run
the following commands to try the LBRY client.
```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak --user install flathub io.lbry.lbry-app
```

## Building
The following is a guide for building `lbry-app` from scratch for `flatpak`
using `flatpak-builder` and the build tools found in the
[`flatpak-builder-tools`](https://github.com/flatpak/flatpak-builder-tools)
repo.

```bash
# Add the flathub repo
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Install Needed SDK, Platform, & Electron Base App
flatpak install flathub org.freedesktop.Sdk
flatpak install flathub org.freedesktop.Platform
flatpak install flathub io.atom.electron.BaseApp

# Clone repo from flathub
git clone https://github.com/flathub/io.lbry.lbry-app.git

# Get the yarn.lock file, replace `MAJOR.MINOR.MICRO` with the targeted release
# version (git tag)
wget https://raw.githubusercontent.com/lbryio/lbry-app/vMAJOR.MINOR.MICRO/yarn.lock

# Build a flatpak ready version of the yarn.lock in flatpak manifest json format
./flatpak-yarn-generator.py yarn.lock -o generated-sources.json

# Build application from manifest (`io.lbry.lbry-app.json`)
# Builds get put into a local build directory (`lbry-repo`) which we access via
# a local repo (`lbry-app`)
flatpak-builder --force-clean --repo=lbry-repo lbry-app io.lbry.lbry-app.json

# Add the new local repo (`lbry-app`) from the local build directory (`lbry-repo`)
flatpak --user remote-add --no-gpg-verify --if-not-exists lbry-app lbry-repo

# Install `io.lbry.lbry-app` from local repo (`lbry-app`)
flatpak --user install lbry-app io.lbry.lbry-app

# Run
flatpak run io.lbry.lbry-app
```
