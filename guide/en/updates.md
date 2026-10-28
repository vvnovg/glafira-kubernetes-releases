# Updates

This chapter covers updating the app, the **About** screen and supporting the project.

<a id="updates"></a>

## Updates

At startup, the app checks once whether a new version is available. If it is, a notice with the version number appears above the tabs. The **Details** link opens the **About** screen. If the check fails, for example without internet access, nothing is shown.

To check manually:

1. Open **About** at the bottom of the left panel.
2. In the **Updates** section, click **Check for updates**.
3. If a new version is available, the screen shows its number and the description of the changes. Click **Install and restart**.

The app downloads the update, verifies its signature, installs it and restarts. If a network failure interrupts the download, it is retried automatically, up to three attempts in total. If all three attempts fail, the app reports: "**The update download was interrupted — check your network and try again**". If the signature fails verification, the app reports: "**The update signature failed verification — the update was not installed**".

Installing may ask for an administrator password, because the app is installed for all users of the computer: by the `.pkg` installer on macOS, the `.msi` installer on Windows, or a `.deb` or `.rpm` package on Linux. Users without administrator rights cannot update such an installation.

If LLM assistants are connected to Glafira over MCP, the app asks first and shows how many there are. Click **Disconnect assistants and update** to disconnect them and continue, or **Cancel**. After the restart, the client connects again; Claude Code may ask you to reconnect the server with `/mcp`. Connections, settings, notes and other data are kept after the update.

Updates are downloaded from the releases page on GitHub. If the check ends with "**Failed to check for updates**", check your network or proxy. "**Updates are not configured in this build**" means that the build was made without the update key, for example a development or test build. In that case, download new versions from the releases page: https://github.com/vvnovg/glafira-kubernetes-releases/releases

Your organization may turn off the update check with a policy or point it to an internal mirror. When the check is off, "**Updates are managed by your organization**" appears in the **Updates** section after you click **Check for updates**. Like "**Updates are not configured in this build**", this message is shown only after the click: the check at startup shows nothing in such cases.

<a id="about"></a>

## About

Open **About** at the bottom of the left panel. The screen shows a short description of the app and these lines:

- **Version** — the version number, and **Build** — the identifier of the source code the app was built from. Give both values when you contact support.
- **System** — the operating system and processor architecture.
- **License** — Apache 2.0; the link opens the license text in the browser.
- **Organization policy** — "**set**" or "**none**". Policy errors are listed below.
- **Data:** — the folder with connections, settings, check results and the MCP log.
- **Logs:** — the folder with log files.

The **Updates** section is on the same screen.

<a id="support"></a>

## Support the project

Glafira is free, and its source code is open under the Apache License 2.0. The project lives on donations. The **Support the project** item at the bottom of the left panel and on the **About** screen opens the support page in the browser: https://github.com/vvnovg/glafira-kubernetes-releases/blob/main/SUPPORT.md
