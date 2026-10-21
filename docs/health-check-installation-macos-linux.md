# Install .NET with C# Doctor on macOS and Linux

C# Doctor checks the SDK and runtimes that C# Dev Kit needs. It can install required components into a managed per-user installation without elevation, or guide you when the workspace uses an installation managed by another tool.

## Do this

1. Run **C#: Check health with C# Doctor**.
2. Select the installation or repair action shown for the blocked requirement.
3. Let C# Doctor complete the managed installation, or follow the manual recovery guidance for your selected .NET host.
4. Select **Recheck**. Use **Reload Window** if C# Doctor requests it.

[Review the shared SDK, runtime, and consent rules](health-check-installation.md).

C# Doctor uses a compatible .NET host selected by the CLI for the workspace. It modifies an installation only when it is authorized to manage that installation. If a package manager, version manager, explicit path, or other user-controlled installation selects `dotnet`, repair it with its owning tool, select a compatible installation, or clear an invalid explicit path override.

## Repair a missing tooling SDK or runtime

**We require:** The SDK selected by the .NET CLI for the workspace must satisfy the workspace and tooling requirements, and its required .NET and ASP.NET Core runtimes must be available.

**Fix it:** Select the installation action shown in C# Doctor when it can manage the selected installation; no administrator access is required for its per-user installation. Otherwise repair the user-controlled installation with its owning tool or correct the workspace's .NET selection.

**Verify:** Select **Recheck**, then run `dotnet --info` in a new terminal and confirm your shell selects the expected installation.

**Still blocked?** See [Tooling Requirements troubleshooting](health-check-troubleshooting.md#tooling-requirements).

## Repair an incompatible `global.json`

**We require:** The `global.json` that governs the first workspace folder must allow the supported tooling SDK.

**Fix it:** Use the proposed C# Doctor repair only if it matches the repository's SDK policy, or ask the repository owner to update `version`, `rollForward`, `allowPrerelease`, or `paths`.

**Verify:** Run `dotnet --info` from the workspace folder, then select **Recheck**.

**Still blocked?** See [Tooling Requirements troubleshooting](health-check-troubleshooting.md#tooling-requirements).

## Install missing project runtimes

**We require:** Every .NET and ASP.NET Core runtime band reported from the projects' evaluated target frameworks must be installed.

**Fix it:** Select **Install missing .NET runtimes** when offered; otherwise install each reported runtime from [Download .NET](https://aka.ms/dotnet-download).

**Verify:** Run `dotnet --list-runtimes`, then select **Recheck**.

**Still blocked?** See [Project Runtime Requirements troubleshooting](health-check-troubleshooting.md#project-runtime-requirements).

## Fix shell and `PATH` selection

**We require:** A new shell must be able to find the intended `dotnet` host and select a compatible SDK from the workspace.

After C# Doctor successfully installs and verifies a `dotnetup`-managed SDK, it offers a separate optional **Configure shell PATH** action. Selecting it uses a stable per-user `dotnetup` executable to run `dotnetup env set shell --dotnetup-on-path true --install-path <verified-root>` and modifies the detected shell profile for future shells. The SDK remains ready if you skip this action or if it fails, and the action does not change the environment of the current VS Code process.

**Fix it:** Select **Configure shell PATH** when offered, then start a new shell and relaunch VS Code from it. As a manual fallback, inspect `which dotnet` and `dotnet --info` in a new shell, and correct your shell startup files if another installation wins `PATH`.

**Verify:** Restart VS Code from the corrected shell environment, select **Recheck**, and reload if requested.

**Still blocked?** See [stale-result troubleshooting](health-check-troubleshooting.md#if-the-view-appears-stuck-or-stale).

> [!NOTE]
> Use these Linux checks only when C# Dev Kit is running in a Linux environment. Launching Windows VS Code from a WSL shell does not by itself validate the Linux installation.
