# xdg-open-simple

Utility script to open a file or URI in the user's preferred application.

This script is a fork of the original `xdg-open` and `xdg-mime` scripts from `xdg-utils` v1.2.1.

Notable properties of this fork are:

-   Doesn't itself forward paths without a known handler to a web browser.
-   Doesn't itself forward paths to any other or DE-specific file openers.
-   Relies solely on the `.desktop` files and `mimeapps.list` information for the file type and URI scheme handling.
-   Handles relative file paths.
-   A lot more concise than the original `xdg-open`.
-   No dependency on `xdg-mime` or other scripts from `xdg-utils`.
-   The applicable error exit codes are retained from the original `xdg-open`.

## Resources

-   [xdg / xdg-utils · GitLab](https://gitlab.freedesktop.org/xdg/xdg-utils)
-   [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/)
