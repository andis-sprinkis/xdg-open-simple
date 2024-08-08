# xdg-open-simple

A fork of the original `xdg-open` (and `xdg-mime`) scripts from the `xdg-utils`.

`xdg-open` is the default utility of opening files and URLs on most Linux desktop systems.

Notable properties of this fork are:

-   [x] Relies solely on the `.desktop`, `mimeapps.list` files information for opening files and URIs.
-   [x] Doesn't auto-forward paths without a known handler to a web browser.
-   [x] Doesn't auto-forward paths to any other or DE-specific file opener.
-   [x] Handles the relative file paths.
-   [x] Has no dependency on the `xdg-utils` scripts.
-   [x] A lot more concise than the original `xdg-open`.

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

---

## How-to's

Tips for dealing with the Linux file-type vs. application associations. Not specific to this project itself - will work with the stock `xdg-utils` too.

### Setting the user default file and URL associations

`$HOME/.config/mimeapps.list`:

```ini
[Default Applications]
x-scheme-handler/msteams=Teams.desktop
video/mp4=video.desktop
...
```

-   [Association between MIME types and applications](https://specifications.freedesktop.org/mime-apps-spec/latest/)

The system `.desktop` files are in `/usr/share/applications`.

### A user custom file handler

`$HOME/.local/share/applications/video.desktop`:

```ini
[Desktop Entry]
Type=Application
Name=Video player
Exec=mpv --player-operation-mode=pseudo-gui -- %U
```

-   [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/)

### Printing the file MIME type

```sh
file --brief --dereference --mime-type ./image.jpg
```

```
image/jpeg
```

The URLs get the `x-scheme-handler/*` MIME type, with the URL prefix as the suffix.

### Printing the known MIME types

```sh
find /usr/share/mime/*/*.xml -type f | sed -e 's/\/usr\/share\/mime\///g' -e "s/\.xml$//g" | less
```

```
application/andrew-inset.xml
application/annodex.xml
application/appinstaller.xml
...
```

### Preventing _Mozilla Firefox_ from rewriting the user's `mimeapps.list` file

```sh
echo > "$HOME/.mozilla/firefox/PROIFLE.default-release/handlers.json'
```

This clears the `handlers.json` file, which removes the downloaded file custom handlers configured in Firefox.
