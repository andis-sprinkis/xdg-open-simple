# xdg-open-simple

A more minimal variant of `xdg-open`.

## Characteristics

- Adheres to the same XDG specifications as the original `xdg-open`
  - ["Desktop Entry Specification"](https://specifications.freedesktop.org/desktop-entry-spec/latest/)
  - ["Association between MIME types and applications"](https://specifications.freedesktop.org/mime-apps-spec/latest/).

- Support for the relative target file paths and URLs has been added.
- Retains the original `xdg-open` exit codes.
- From `xdg-utils` substitutes only the `xdg-open`, not the other tools.
- **Some of the original `xdg-open` functionality has been REMOVED:**

    - Auto-forwarding target paths without a known MIME handler to a web browser. (Not in specifications)

        Reasons:

        1. Obfuscates that there is a missing MIME handler on the system.
        1. Unexpected and needless behavior - the web browser creating a copy of the target file in the downloads directory.

    - Handling the `$BROWSER` variable. (Not in specifications)

        Reasons:

        1. A legacy convention of setting the default web browser on some environments.
        1. Redundant, overlaps with the core functionality of `xdg-open`.

    - Handling any `mimeapps.list` and Desktop file search paths that have been deprecated. (In specifications)
    - The desktop environment specific integrations:

        1. Auto-forwarding target paths to desktop environment specific file openers. (Not in specifications)
        1. Use of the file `<desktop environment name>-mimeapps.list`. (In specifications)
        1. Use of the file `<desktop environment name>-mimeinfo.cache`. (Not in specifications)

        Reasons:

        1. Lack of a robust identification of the desktop environments and their the default opener commands.

           These features have required adding lots of work-arounds in the original `xdg-open`.
        1. Not required for the core functionality of this tool.

    - Substituting `Name` (`%c`) and `Icon` (`%i`) field codes within the `Exec` key to pass program icon identifiers and localized program names to the applications. (In specifications)

        Reason:

        1. Unlikely to be in use - out of more than 100 Desktop files on my Linux system, none use these field codes.

## Resources

- [xdg / xdg-utils · GitLab](https://gitlab.freedesktop.org/xdg/xdg-utils)
- [xdg-utils on freedesktop.org](https://www.freedesktop.org/wiki/Software/xdg-utils/)
- [Association between MIME types and applications](https://specifications.freedesktop.org/mime-apps-spec/latest/)
- [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/)
- [xdg-utils - ArchWiki](https://wiki.archlinux.org/title/Xdg-utils)
