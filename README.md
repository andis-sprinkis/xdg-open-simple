# xdg-open-simple

`xdg-open-simple` is a more minimal variant of the `xdg-open`.

## Properties of this fork

- **Not** an attempt to reinvent what the `xdg-open`.

    Adheres to the same XDG specifications as the original `xdg-open`: ["Desktop Entry Specification"](https://specifications.freedesktop.org/desktop-entry-spec/latest/), ["Association between MIME types and applications"](https://specifications.freedesktop.org/mime-apps-spec/latest/).

- **Some of the original `xdg-open` functionality has been REMOVED:**

    - Auto-forwarding target paths without a known MIME handler to a web browser. (Not in specifications.)

        Reasons:

        1. Obfuscates that there is a missing MIME handler on the system.
        1. If the web browser can't render the target file, results in creating a needless copy of it in the browser downloads directory.

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
- From `xdg-utils` system package substitutes only the `xdg-open`, not the other tools. Retains the original `xdg-open` exit codes.

## Resources

- [xdg / xdg-utils · GitLab](https://gitlab.freedesktop.org/xdg/xdg-utils)
- [xdg-utils on freedesktop.org](https://www.freedesktop.org/wiki/Software/xdg-utils/)
- [Association between MIME types and applications](https://specifications.freedesktop.org/mime-apps-spec/latest/)
- [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/)
- [xdg-utils - ArchWiki](https://wiki.archlinux.org/title/Xdg-utils)
