# xdg-open-simple

Utility script to open a file or URI in the user's preferred application.

A fork of the original `xdg-open` and `xdg-mime` scripts from the `xdg-utils` version 1.2.1.

Notable properties of this fork are:

-   [x] A lot more concise than the original `xdg-open`.
-   [x] Relies solely on the `.desktop`, `mimeapps.list` files information for opening files and URIs.
-   [x] Doesn't auto-forward paths without a known handler to a web browser.
-   [x] Doesn't auto-forward paths to any other or DE-specific file opener.
-   [x] Has no dependency on the `xdg-utils` scripts.
-   [x] The applicable error exit codes are retained from the original `xdg-open`.
-   [x] Handles the relative file paths.

## Resources

-   [xdg / xdg-utils · GitLab](https://gitlab.freedesktop.org/xdg/xdg-utils)
-   [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/)
