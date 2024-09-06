# xdg-open-simple

`xdg-open-simple` is a minimal fork of the `xdg-open` - the default utility of opening files and URLs on many Linux & Unix-like desktop operating systems.

## Notable properties of this fork

-   **Several features and behaviors have been removed:**
    -   Auto-forwarding target file paths without a known handler to a web browser.
    -   Handling the `$BROWSER` variable.
    -   Handling any `mimeapps.list` and Desktop file search paths that have been deprecated.
    -   Auto-forwarding target file paths to desktop environment specific file openers.
    -   Handling of `${XDG_DESKTOP_SESSION}-mimeapps.list`.
        -   Reason: the lowercased `$XDG_DESKTOP_SESSION` value in several desktop environments doesn't map directly to the `xdg-open` search paths in the simple way it's described in the specification.
    -   Substituting `Name` (`%c`) and `Icon` (`%i`) field codes within the `Exec` key.
        -   Reason: no application vendor uses this - out of the 100 Desktop files on my Linux system, none have these field codes in `Exec`.
    -   Fallback to `mimeinfo.cache` for finding MIME-associated Desktop file IDs.
        -   Reason: not in the specification and suggests a Desktop files MIME info discrepency, which should be surfaced.
-   Support of the relative target file paths has been added.
-   Has no dependency on the `xdg-utils` and is **a lot more concise than the original** `xdg-open`.
-   Retains the original `xdg-open` exit codes, where applicable, for compatibility.
-   Adheres to the XDG specifications ["Desktop Entry Specification"](https://specifications.freedesktop.org/desktop-entry-spec/latest/), ["Association between MIME types and applications"](https://specifications.freedesktop.org/mime-apps-spec/latest/).

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

-   [xdg / xdg-utils · GitLab](https://gitlab.freedesktop.org/xdg/xdg-utils)
-   [Association between MIME types and applications](https://specifications.freedesktop.org/mime-apps-spec/latest/)
-   [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/)

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
