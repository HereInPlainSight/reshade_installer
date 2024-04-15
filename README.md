# reshade_installer

This is a CLI [ReShade](https://reshade.me/) installer for Linux.  It both downloads and updates ReShade and can be used to install / update your ReShade installs for individual games that will be run through WINE.

## Getting Started

By default, at this time, there are no included presets.  Please see below.

Once you've got the script and it's executable, you can simply run it.  There is a basic menu.

If you would prefer to run the commands directly, the help menu should give you the basics (`./reshade_installer.sh --help`):
```
Syntax options:
                                ./reshade_installer.sh                                           -- Guided tutorial
                                ./reshade_installer.sh update [force|shaders|presets]            -- Install / Update to latest ReShade, or update shaders / presets.  Optionally force the ReShade update.
                                ./reshade_installer.sh list                                      -- List games, numbers provided are for use with remove / delete options.
                                ./reshade_installer.sh remove <#>                                -- Remove <#> from database, leave ReShade in whatever shape it's currently in.
                                ./reshade_installer.sh delete <#>                                -- Delete ReShade from <#> and remove from database.
<WINEPREFIX=/path/to/prefix>    ./reshade_installer.sh ffxiv                                     -- Install to FFXIV in provided Wine Prefix or autodetect if no Wine Prefix.
<WINEPREFIX=/path/to/prefix>    ./reshade_installer.sh [dx(?)|opengl] /path/to/game.exe          -- Install to custom location with designated graphical API version. 'dxgi' is valid here (and the recommended default).

                                                                        Note: game.exe should be the GAME'S .exe file, NOT the game's launcher, if it has one!
                                                                        WINEPREFIX is only required for opengl games!
```
You can clone the repo and run the script from within it.  Instructions to do so are below the prerequisites.

### Prerequisites

Universally required:
* `awk`, `curl`, `find`, `hash`, `ln`, `perl`, `sed`, `unzip`, `rsync`

Additional requirements for Linux:
* `7z`, `md5sum`

Additional requirements for Mac:
* `ditto`, `md5`

### Installation

There's multiple ways to run the installer, this is just one method.
  1) Clone the repo:  `git clone https://github.com/HereInPlainSight/reshade_installer.git`
  2) Change directory into it:  `cd reshade_installer`
  3) Run the script:  `./reshade_installer.sh`

We install to the `$XDG_DATA_HOME/ReShade/` directory, which defaults to `$HOME/.local/share/ReShade/`.

### Adding presets

*No* presets are currently included by default.

After initial installation, you may add files in the installation (default location: `$HOME/.local/share/ReShade/`) directory's `include.d` and `blacklist.d` to have them included / excluded from your local collections, respectively.  Both [.preset](https://github.com/HereInPlainSight/reshade_installer/blob/main/include.d/01-ipsusu.preset) and [.shader](https://github.com/HereInPlainSight/reshade_installer/blob/main/include.d/01-ipsusu.shader) files linking to repositories (or zip files) are processed unless the line begins with a `#`.  `RepositoryUrl`s found in [ReShade's EffectPackages.ini](https://github.com/crosire/reshade-shaders/blob/list/EffectPackages.ini) are included automatically, but may be added to the `blacklist.d` folder.  Please see the [example file](https://github.com/HereInPlainSight/reshade_installer/blob/main/blacklist.d/01-disabledExample.shader).

### Updating

When updating ReShade through the script (`./reshade_installer.sh update` or through the guided menu's `1` option), *all* existing installs are updated if an update is found.

#### ***If any of the following seems complicated -- just run `./reshade_installer.sh` and follow the guided prompts.***

If you're installing ReShade for use with FFXIV, you can try the auto-installer by running `./reshade_installer.sh ffxiv`, which will search for a default-location Steam install (the default WINE prefix in Steam is `$HOME/.steam/steam/steamapps/compatdata/39210/pfx`, and the game itself is in `$HOME/.steam/steam/steamapps/common/FINAL FANTASY XIV Online/game/`), a Lutris install (by looking for a configuration file), the default XLCore location, or by checking a provided WINEPREFIX.

If you wanted to install ReShade to a Steam install of FFXIV manually, you would use the following command:
```
./reshade_installer.sh dxgi "$HOME/.steam/steam/steamapps/common/FINAL FANTASY XIV Online/game/ffxiv_dx11.exe"
```

If you're having any problems, you can check `./reshade_installer.sh debug`, which will print something similar to this:
![Debug example](https://i.imgur.com/5O1jJOZ.png)
Green is good.  Red indicates an error with that component.  Yellow is informational, such as a 'hard install' without symlinks.  Neither good nor bad -- just something to be aware of when troubleshooting.  WINEPREFIX will be recorded if available for non-openGL games, but is only necessary for openGL games.

You can get just the games.db list by requesting it with `./reshade_installer.sh list`.  You can remove an item from the list with `./reshade_installer.sh remove <#>`, or delete the ReShade installation from the game with `./reshade_installer.sh delete <#>`.

If you're having trouble and asking a friend for help, you can use `./reshade_installer.sh debug upload` and give your friend the URL.  They can use the URL with `curl <URL>` to see the exact output you would see if you ran the debug yourself.

## Troubleshooting

Q: I'm on a Mac --

A: Anything Mac-related was contributed Back-In-The-Day by the good people at [XIV on Mac](https://www.xivmac.com/).  If your question is XIV-specific, you'll almost *certainly* want to contact them directly.  Present status of script on Mac is 'untested and unsupported, but who knows it might work?'

Q: I'm having an issue with...

A: Great!  Please check the [issues](https://github.com/HereInPlainSight/reshade_installer/issues) tracker and join an existing discussion if it's already there or create a new one if it's not.  Contributions are welcome.  Most of this is pretty new and has had limited testing so far!

## Contributors

If you are a code contributor, please feel free to add yourself to the list during your commit!  My memory is roughly sieve-shaped!

* [@JacoG-RH](https://github.com/JacoG-RH) - Non-standard Steam libraries.
* [@Maia-Everett](https://github.com/Maia-Everett) - Support for Wine Steam, locating game via wine registry, and `$HOME/.wine` checking.
* [@taylor85345](https://github.com/taylor85345) - XIVLauncher.Core auto-detection.
* [@FleetAdmiralButter](https://github.com/FleetAdmiralButter) - Non-AVX detection. (Presently unused but if ReShade starts using, guess it'll be back)
* [@marzent](https://github.com/marzent) - All the work to add Mac support.
* [@Zoeyrae](https://github.com/Zoeyrae) - XIV on Mac support to the auto installer.

