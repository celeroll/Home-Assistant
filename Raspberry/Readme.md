## Install OSMC

Download image from [osmc.tv](http://osmc.tv) and install it with `osmc installer`.
- Wifi should be setup in the `osmc installer`
- SSH is activated on default. User: `osmc`, Pass: `osmc`

### Install Harmony remote / hub

This can be tricky. But the main idea is to add new harmony device, so that "bluetooth pairing" appear,  and OSMC can see the harmony keyboard as bluetooth device.
- Manufacturer: `Microsoft`
- Device: `Media Player`

After the whole pairing process, "old" activity should also work and new added  devices can be removed from harmony app.

---
Samba should be installed from `My OSMC` without problems.

## Setup KODI

### Plex
For access to a plex server, I use Kodi add on `PlexKodiConnect`.
- File for download can be found here: [PlexKodiConnect](files/).
- Like to source is here:
[PlexKodiConnect GitHub](https://github.com/croneter/PlexKodiConnect)

## Skin
I have used a skin "Xonfluence", because the menu can be fully customized.
- File for download can be found here: [hellyrepo.kodi](files/).
- Like to source is here:
[Xonfluence GitHub](https://github.com/Helly1206/skin.xonfluence)

- Menu can be edited directly in the `settings.xml` file:
 `C:\Users\XXX\AppData\Roaming\Kodi\userdata\addon_data\skin.xonfluence`
- However, it is important, that the skin is not active, when editing the file. Skin has to be disabled first. Otherwise, the changes will not be taken.


## Toggle fullscreen / window mode
To be able to map a mouse wheel for a specific Kodi command, it is important, that the file is saved here:

- `C:\Users\XXX\AppData\Roaming\Kodi\userdata\keymaps`

- An example for such a file can be found here:
[togglefullscreen.xml](files/)
- Another example of advanced settings: [Kodi mouse alternative settings](http://kodi.wiki/view/Alternative_keymaps_for_mice)

## BG Repository
For BG subtitles, there is a repository hosted by `martinstz`.
- File for download can be found here: [mar33](files/).

- Like to source is here (always look for the last post):
[Mar33 BG repository](https://kodibg.org/forum/post-8946.html)

## Ambylight

## Picons for satelites
https://linuxsat-support.com/thread/124700-picons-by-chocholou%C5%A1ek/
