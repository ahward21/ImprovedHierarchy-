# Improved Hierarchy — All-In-One bundle (0.2.10)

This bundle contains **two** UPM packages designed to be installed together via Unity's **"Install package from disk..."** workflow:

| Folder                                  | Package id                                | Version | Purpose                                                                                              |
|-----------------------------------------|-------------------------------------------|---------|------------------------------------------------------------------------------------------------------|
| `com.improvedhierarchy/`                | `com.improvedhierarchy`                   | 0.2.10  | Hierarchy / Inspector / Project window improvements, hierarchy tree lines, git-aware folder badges.  |
| `com.handzlikchris.fastscriptreload/`   | `com.handzlikchris.fastscriptreload`      | 1.6.1   | Optional hot-reload integration (MIT, Chris Handzlik — unmodified upstream + small Unity-6 patches). |

Improved Hierarchy works standalone. When FSR is installed alongside it, the `⚡ Hot ON/OFF` badge appears in the hierarchy toolbar and the FSR bridge handles auto-silencing of its first-run dialogs.

## Compatibility

- Unity **2022.3 LTS** and newer
- Unity **6.0, 6.1, 6.2, 6.3** (legacy `GetInstanceID` API path)
- Unity **6.4+** (new `EntityId` API path, picked up automatically — compiles warning-free)
- VCS badges require **git** on the system PATH and the project to be inside a git repository. The provider self-disables silently when those preconditions aren't met.

## Install — recommended path ("Install package from disk")

This bundle ships each package as a **folder with a `package.json` at its root**, which is exactly what Unity's `Install package from disk` action expects.

1. Unzip this bundle somewhere stable — for example `C:\UnityPackages\` or directly inside your Unity project's `Packages/` folder. **Do not move the folders after installing** unless you want to re-link them; Unity keeps a `file:` reference to wherever you point it.
2. In Unity, open `Window → Package Manager`.
3. Click the **`+`** dropdown in the top-left → **`Install package from disk...`**.
4. Navigate into `com.handzlikchris.fastscriptreload/` and select its `package.json`. Click **Open**.
5. Wait for the import to finish.
6. Repeat steps 3-4 for the second package: `com.improvedhierarchy/package.json`.

Order matters slightly — installing FSR first means the `⚡ Hot ON` badge lights up immediately on the Improved Hierarchy install instead of needing one extra recompile.

### Want it to feel even more "embedded"?

Drop both folders into your Unity project's `Packages/` directory **before** the project is opened. Unity auto-detects them as embedded packages — no Package Manager UI needed. The result is identical to `Install from disk` pointing at the same folders, but persists with the project.

## Alternative install paths

### Via tarballs (`tarballs/*.tgz`)

If you'd rather not keep the unpacked folders around, this bundle also ships pre-built `.tgz` tarballs in `tarballs/`:

1. `Window → Package Manager → + → Install package from tarball...`
2. Pick `tarballs/com.handzlikchris.fastscriptreload-1.6.1.tgz`.
3. Repeat for `tarballs/com.improvedhierarchy-0.2.10.tgz`.

The tarballs are self-contained — Unity unpacks them into its own package cache, so you can delete the tarball after a successful install.

### Improved Hierarchy only (no hot reload)

Skip the FSR install entirely. Improved Hierarchy detects FSR's absence at startup and silently hides the Hot Reload badge plus the `Tools › Improved Hierarchy › Fast Script Reload` menu items.

## Upgrading from an older Improved Hierarchy (< 0.2.8) — read this

Versions **before 0.2.8** stored per-GameObject icon/colour data on an `IconData` MonoBehaviour attached to your scene objects. Removing the package back then left a `Missing (Mono Script)` reference on every customised object.

0.2.8+ fixes this permanently — per-object data now lives **externally** in `Assets/Editor/InspectorIconLibrary.asset` (keyed by `GlobalObjectId`), so the package never touches your GameObjects. On upgrade, any legacy `IconData` is migrated automatically the first time a customised object is drawn.

For a guaranteed-clean project (recommended before ever uninstalling):

- `Tools › Improved Hierarchy › Migrate Legacy IconData → Library` — one-shot pass over every scene.
- `Tools › Improved Hierarchy › Clean Missing Scripts in Open Scenes` — strips any dangling missing-script references left by a pre-0.2.8 removal.

After migration, uninstalling the package only leaves the single `InspectorIconLibrary.asset` behind (delete it if you want a 100% clean removal).

## What Improved Hierarchy adds (highlights)

- Hierarchy
  - Per-GameObject custom icons + soft-fade row tint that doesn't tint the icon or text.
  - **NEW in 0.2.9** — parent→child **tree lines** in the indentation gutter. **0.2.10** brightened them and re-anchored the rails under the parent column so foldout arrows no longer cover them. Toggle via `Tools › Improved Hierarchy › Hierarchy Lines`.
  - Component icon strip with identical-type grouping and an overflow indicator. **0.2.10** packs the strip into the space left after the name, so icons no longer pile on top of the label on a narrow window/pane.
  - Top-of-hierarchy scene selector dropdown (search + favourites, native PopupWindow).
  - `⚡ Hot ON/OFF` badge when FSR is installed — left-click toggle, right-click manual reload.
  - IconLibrary ships pre-populated with ~35 curated default favourites; **0.2.9** makes them appear the first time you open the icon picker (no add/remove dance needed).
- Inspector
  - Save / Multi-Copy buttons on every component header.
  - `Tab`, `Foldout`, `Button`, `ShowIf`, `ReadOnly`, `ProgressBar`, `MinMaxSlider`, `InfoBox`, `Required` attribute drawers.
- Project window
  - Folder colour + custom icon support (list and grid views).
  - `Customize Folder...` opens as a native dropdown popup anchored to the cursor.
  - Git-aware VCS badge in the top-right of every folder icon: **yellow** for uncommitted changes, **red** for untracked files, no badge for clean folders. Toggle via `Tools › Improved Hierarchy › VCS Folder Badges`, force refresh with `Ctrl+Alt+V`.
- Pickers
  - Right-click an icon in either picker to remove it from favourites in one click.
- Fast Script Reload integration
  - Reflection-only bridge — no hard compile dependency between the two packages.
  - Auto-heals Unity's `kAutoRefreshMode` if FSR's welcome dialog disables asset auto-refresh.
  - Suppresses FSR's welcome screen auto-launch + the auto-refresh warning log/dialog while leaving copyright + author credit intact.

## Updating later

Improved Hierarchy and Fast Script Reload are independent packages — you can update one without touching the other. Replace the corresponding folder (or rerun `Install package from disk` against a newer copy of `package.json`) and Unity picks up the change on the next recompile.

## Credits & licensing

- **Improved Hierarchy** — MIT, see `com.improvedhierarchy/LICENSE.md`.
- **Fast Script Reload** — MIT, Copyright (c) 2022 Chris Handzlik. Bundled here in unmodified form except for small Unity-6 compatibility patches, each annotated with `[Improved Hierarchy patch]` and preserving the original code in comments. Upstream: https://github.com/handzlikchris/FastScriptReload
