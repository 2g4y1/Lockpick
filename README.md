Lockpick
=
This is a ground-up C++17 rewrite of homebrew key derivation software, namely [kezplez-nx](https://github.com/tesnos/kezplez-nx). It also dumps titlekeys. This will dump all keys through `*_key_05` on firmwares below 6.2.0 and through `*_key_06` on 6.2.0 and above.

What this software does differently
=
* Dumps titlekeys
* Uses the superfast `xxHash` instead of `sha256` when searching exefs for keys for a ~5x speed improvement
* Gets all possible keys from running process memory - this means no need to decrypt `Package2` at all, let alone decompress `KIP`s
* Gets `header_key` without `tsec`, `sbk`, `master_key_00` or `aes` sources - which may or may not be the same way `ChoiDujourNX` does it :eyes: (and I'm gonna issue a challenge to homebrew title installers to implement similar code so you don't need your users to use separate software like this :stuck_out_tongue_winking_eye: it's up to you to figure out if the same can be done for `key_area_keys` if needed)

Usage
=
1. Use [Hekate](https://github.com/CTCaer/hekate/releases) to dump TSEC and fuses:
    1. Push hekate payload bin using [TegraRCMSmash](https://github.com/rajkosto/TegraRcmSmash)/[TegraRCMGUI](https://github.com/eliboa/TegraRcmGUI)/modchip/injector
    2. Using the `VOL` and `Power` buttons to navigate, select `Console info...`
    3. Select `Print fuse info`
    4. Press `Power` to save fuses to SD card
    5. Select `Print TSEC keys`
    6. Press `Power` to save TSEC to SD card
2. Launch CFW of choice
3. Open `Homebrew Menu`
4. Run `Lockpick`
5. Use the resulting `prod.keys` file as needed and rename if required

Building
=
Release built with `libnx v1.6.0`.

Uses `freetype` which comes with `switch-portlibs` via `devkitPro pacman`:
```
pacman -S libnx switch-portlibs
```
then run:
```
make
```
to build.

Special Thanks
=
* tèsnos! For making [kezplez-nx](https://github.com/tesnos/kezplez-nx), being an all-around cool and helpful person and open to my contributions, not to mention patient with my *enthusiasm*. kezplez taught me an absolute TON about homebrew.
* SciresM for [hactool](https://github.com/SciresM/hactool), containing to my knowledge the first public key derivation *software*, and for `get_titlekeys.py`
* roblabla for the original keys [gist](https://gist.github.com/roblabla/d8358ab058bbe3b00614740dcba4f208) and for believing in our habilities
* The folks in the [ReSwitched](https://reswitched.team/) Discord server for answering my innumerable questions while researching this (and having such a useful chat backlog!)
* The memory reading code from jakibaki's [sys-netcheat](https://github.com/jakibaki/sys-netcheat) was super useful for getting keys out of running process memory
* The System Save dumping methodology from Adubbz' [Compelled Disclosure](https://github.com/Adubbz/Compelled-Disclosure)
* Shouts out to fellow key derivers: shadowninja108 for [HACGUI](https://github.com/shadowninja108/HACGUI), Thealexblarney for [Libhac](https://github.com/Thealexbarney/LibHac), and [rajkosto](https://github.com/rajkosto/) :eyes:
* The constantly-improving docs on [Switchbrew wiki](https://switchbrew.org/wiki/) and [libnx](https://switchbrew.github.io/libnx/files.html)
* [mission2000](https://github.com/misson20000) for help with `std::invoke` to get the function timer working
* Literally the friends I made along the way! I came to the scene late and I've still managed to meet some wonderful people :) Thanks for all the help testing, making suggestions, and cheerleading!

Licenses
=
* `AES` functions are from [mbedtls](https://tls.mbed.org/) licensed under [GPLv2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html))
* `creport_debug_types` and fast `sha256` implementation are from [Atmosphère](https://github.com/atmosphere-NX/Atmosphere) licensed under [GPLv2](https://github.com/Atmosphere-NX/Atmosphere/blob/master/LICENSE)
* Simple `xxHash` implementation is from [stbrumme](https://github.com/stbrumme/xxhash) licensed under [MIT](https://github.com/stbrumme/xxhash/blob/master/LICENSE)
* Padlock icon is from [Icons8](https://icons8.com/) licensed under [Creative Commons Attribution-NoDerivs 3.0 Unported](https://creativecommons.org/licenses/by-nd/3.0/)