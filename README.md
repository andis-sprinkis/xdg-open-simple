# xdg-open-simple

`xdg-open-simple` is a minimal fork of the `xdg-open`, which is the default utility of opening files and URLs with the right application on many Linux & Unix-like operating systems.

## Properties of this fork

- **Not** an attempt to reinvent how `xdg-open` is meant to work.

    Adheres to the same XDG specifications as the original `xdg-open`: ["Desktop Entry Specification"](https://specifications.freedesktop.org/desktop-entry-spec/latest/), ["Association between MIME types and applications"](https://specifications.freedesktop.org/mime-apps-spec/latest/).

- **Some of the original `xdg-open` features and behaviors have been REMOVED:**

    - Auto-forwarding target paths without a known MIME handler to a web browser. (Not in specifications.)

        Reasons:

        1. Obfuscates that there is a missing MIME handler on the system.
        1. Often just results in creating the target file copies in the browser downloads directory.

    - Handling the `$BROWSER` variable. (Not in specifications.)

        Reasons:

        1. A legacy convention of setting the default web browser on some Linux systems.
        1. Redundant, overlaps with the core functionality of `xdg-open`.

    - Handling any `mimeapps.list` and Desktop file search paths that have been deprecated. (In specifications.)
    - Desktop environment specific integrations:

        - Auto-forwarding target paths to desktop environment specific file openers. (Not in specifications.)
        - Use of `<lowercased DE name>-mimeapps.list` (in specifications), `<lowercased DE name>-mimeinfo.cache` (not in specifications).

        Reasons:

        1. Temporal work-arounds and data mappings are needed to identify the desktop environments, due to the lack of desktop environment vendor adherence to, or the lack of the definition in the XDG specifications.
        1. Not required for providing the core functionality of this tool.

    - Substituting `Name` (`%c`) and `Icon` (`%i`) field codes within the `Exec` key to pass program icon identifiers and localized program names to the applications. (In specifications.)

        Reason:

        1. Unlikely to be in use anymore. Out of the 100 Desktop files on my Linux system, none use these field codes.

- Support of the relative target file paths and URLs has been added.
- From `xdg-utils` system package substitutes only the `xdg-open`, not it's other tools. Retains the original applicable `xdg-open` exit codes.

## Installation

Add this line to some shell initialization script (e.g. `$HOME/.profile` for Bash, `$HOME/.zshenv` for ZSH):

```sh
PATH="$HOME/.local/bin:$PATH"
```

Run:

```sh
mkdir -p "$HOME/.local/opt/" "$HOME/.local/bin"
git clone "https://github.com/andis-sprinkis/xdg-open-simple/" "$HOME/.local/opt/xdg-open-simple"
cd "$HOME/.local/bin"
ln -s "../opt/xdg-open-simple/xdg-open" "xdg-open"
```

## Resources

- [xdg / xdg-utils · GitLab](https://gitlab.freedesktop.org/xdg/xdg-utils)
- [Association between MIME types and applications](https://specifications.freedesktop.org/mime-apps-spec/latest/)
- [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/)
- [xdg-utils - ArchWiki](https://wiki.archlinux.org/title/Xdg-utils)

---

### How-to's

Tips for dealing with the Linux file-type vs. application associations. Not specific to this project itself - will work with the stock `xdg-utils` too.

#### Setting the user default file and URL associations

`$HOME/.config/mimeapps.list`:

```ini
[Default Applications]
x-scheme-handler/msteams=Teams.desktop
video/mp4=video.desktop
...
```

The system `.desktop` files are in `/usr/share/applications`.

#### A user custom file handler

`$HOME/.local/share/applications/video.desktop`:

```ini
[Desktop Entry]
Type=Application
Name=Video player
Exec=mpv --player-operation-mode=pseudo-gui -- %U
```

#### Printing the file MIME type

```sh
file --brief --dereference --mime-type ./image.jpg
```

```
image/jpeg
```

The URLs get the `x-scheme-handler/*` MIME type, with the URL prefix as the suffix.

#### Printing the known MIME types

```sh
find /usr/share/mime/ -type f -name '*.xml' | sed -e 's/\/usr\/share\/mime\///g' -e "s/\.xml$//g" | less
```

```
application/andrew-inset.xml
application/annodex.xml
application/appinstaller.xml
...
```

#### Preventing _Mozilla Firefox_ from rewriting the user's `mimeapps.list` file

```sh
echo > "$HOME/.mozilla/firefox/PROFILE.default-release/handlers.json'
```

This clears the `handlers.json` file, which removes the downloaded file custom handlers configured in Firefox.

## Resources

- [xdg-utils on freedesktop.org](https://www.freedesktop.org/wiki/Software/xdg-utils/)
- [xdg / xdg-utils · GitLab](https://gitlab.freedesktop.org/xdg/xdg-utils)
