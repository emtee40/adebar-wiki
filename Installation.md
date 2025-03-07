## Requirements
* **ADB** installed (and configured for your device) on your computer. This can either be the [complete Android SDK](https://developer.android.com/sdk/index.html "Android SDK at Android Developers"), or a [minimal installation of ADB](https://android.stackexchange.com/q/42474/16575 "Android.SE: Is there a minimal installation of ADB?").
* **Bash** (version 4 or higher). As this is a very common shell environment, it's available by default on most Linux distributions.
    * on Linux, this should ship per default
    * on Windows, you can use [Cygwin](https://www.cygwin.com/) (confirmed); [MobaXterm](https://mobaxterm.mobatek.net/) should do as well (no reports yet; includes Cygwin and is easier to install, even offers a portable version)
    * MacOS ships with an ancient version of Bash (3.5). You can use [Macports](https://www.macports.org/) to install a newer version. This doesn't overwrite the current version, just installs the newer version in a different location. Once it's installed, Adebar will use it by default, thanks to d5ve's contribution.
* **Android 4.0+**: As the `adb backup` and `adb restore` commands have not been present before Android 4.0, *Adebar* will not be of much use with devices running older versions – except for creating a „device documentation“, which turned out to work even with Android 2.1.

*Adebar* will work with Android versions before 4.0 – for [[device documentation|example deviceInfo.md]] and [[list of installed apps|example userApps.md]], and maybe even some more. Though it creates the backup and restore scripts as well, those will be of no use – as `adb backup` and `adb restore` have only be added with Android 4.0.


## Obtaining *Adebar*
There are multiple ways to get hold of a copy of the code:

### Sources for manual installation
* you can clone the Git repository (see the [main page](https://codeberg.org/izzy/Adebar "Adebar at Codeberg"))
* you can download the repository code as `.zip` file (from the same place)
* you can download the latest release from the [IzzyOnDroid Download Area](https://android.izzysoft.de/downloads "IzzyOnDroid Download Area")

The `master` branch here at Codeberg should always reflect the latest „stable“ code. The `devel` branch might hold newer stuff which was not yet much tested. The choice is entirely yours :)

### Via the tools of your distribution
* if you're using ArchLinux, you can directly install *Adebar* [from AUR](https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=adebar)
* if you're using Gentoo, you can install Adebar from the [official ebuild repository](https://packages.gentoo.org/packages/app-mobilephone/adebar) using `emerge adebar`

When installed this way, you can skip below installation instructions: *Adebar* is already completely installed, and you can head over to perform its [[Configuration]].


## Installation from source
Just unpack the contents of the downloaded `.zip`/`.tar.gz` file into an empty directory of your chosing, no special installation required. If you cloned the repo, you can run `adebar-cli` directly from within your clone, and also create your `config/` and `cache/` directories there (these are contained in the `.gitignore` file, and thus should cause no conflicts).

**Note:** when cloning the repo on Windows (native) for use in Cygwin, take care to set `autocrlf = false` before cloning, or the resulting files are unusable ([see here](https://codeberg.org/izzy/Adebar/issues/7#issuecomment-245275208) for details).
