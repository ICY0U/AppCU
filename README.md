# AppCU — Application Usage Tracker

A lightweight desktop application that automatically tracks how much time you spend in your applications. Runs quietly in your system tray and logs every session.

**Version:** 1.0.0 · **Platform:** Windows · **License:** MIT

---

## Features

- **Automatic tracking** — detects when tracked apps open and close, logs session times automatically
- **Dashboard** — 6 stat cards showing total time, apps tracked, most used app, active count, today's usage, and average session length
- **9 themes** — Light, Dark, Nord, Solarized, Cyberpunk, Ocean, Forest, and two macOS-style themes with traffic light buttons
- **System tray** — runs in the background, shows active app count, pause/resume tracking
- **Export & Import** — save your data as CSV or JSON, import it on another machine
- **Session history** — per-app table of every session with start/end times and duration
- **Searchable app list** — filter tracked apps, collapsible sidebar, running indicators
- **SQLite storage** — all data stored locally in `appcu.db`

---

## Getting Started

### Download

1. Grab the latest `AppCU-v1.0.0-win64.zip` from [Releases](../../releases)
2. Extract the zip to any folder
3. Run `AppCU.exe`

No installer needed — all dependencies are bundled.

### Adding Apps to Track

1. Click **Add App** in the toolbar
2. Choose from two methods:
   - **Running Processes** — select from a list of currently running apps
   - **Browse** — pick any `.exe` file on your system
3. Give it a display name (auto-filled from the process name)
4. Click **OK** — tracking starts immediately

### How Tracking Works

AppCU polls your running processes every 5 seconds (configurable). When a tracked app is detected running, a session begins. When it closes, the session ends and the duration is recorded.

- Sessions survive crashes — orphaned sessions are recovered on next startup
- All active sessions are properly closed on quit

---

## Dashboard

The Dashboard tab shows six stat cards at a glance:

| Card | Shows |
|------|-------|
| **Total Time** | Combined time across all tracked apps |
| **Apps Tracked** | Number of applications you're tracking |
| **Most Used** | The app with the highest total usage time |
| **Active Now** | How many tracked apps are currently running |
| **Today** | Total tracked time for today |
| **Avg Session** | Average session duration across all sessions |

The dashboard auto-refreshes every 10 seconds.

---

## Themes

Switch themes in **Settings → Appearance**. Changes apply instantly.

| Category | Themes |
|----------|--------|
| **Basic** | Light, Dark, Nord, Solarized Light |
| **Detailed** | Cyberpunk, Midnight Ocean, Forest |
| **macOS** | macOS Light, macOS Dark |

The macOS themes replace the window title bar with Apple-style traffic light buttons (close, minimise, maximise) and support full window dragging and resizing.

Use the 🌙/☀️ button in the toolbar to quickly toggle between Light and Dark.

---

## System Tray

AppCU minimises to the system tray when you close the window. Right-click the tray icon for:

- **Show Dashboard** — bring the window back
- **Currently Tracking** — see which apps are active right now
- **Pause All / Resume All** — temporarily stop or restart tracking
- **Quit** — fully exit the app (all active sessions are saved)

The tray tooltip shows how many apps are currently active.

---

## Export & Import

Back up or transfer your data via **Settings → Data**:

| Format | What's Exported |
|--------|----------------|
| **CSV** | `app_name, executable_name, start_time, end_time, duration_seconds` |
| **JSON** | Structured format with version field and session array |

Importing creates any apps that don't already exist and merges session data.

---

## Settings

Open **Settings** from the toolbar.

| Page | Options |
|------|---------|
| **General** | Launch on startup · Start minimised · Minimise to tray |
| **Appearance** | Theme selection with live preview |
| **Tracking** | Poll interval (1–60 sec) · Auto-track new apps |
| **Data** | Export/Import CSV and JSON |

All settings persist across restarts.

---

## Building from Source

### Requirements

- **CMake** ≥ 3.16
- **C++17** compiler (MSVC 2019+ or GCC/Clang)
- **Qt 6** (Core, Widgets, Sql modules)

### Build

```bash
git clone https://github.com/your-username/AppCU.git
cd AppCU
mkdir build && cd build
cmake ..
cmake --build . --config Release
```

The build automatically runs `windeployqt` to bundle Qt DLLs into the `Release/` folder.

### Run Tests

```bash
cmake .. -DBUILD_TESTS=ON
cmake --build . --config Release
ctest --output-on-failure
```

---

## Project Structure

```
src/
├── main.cpp                    Entry point
├── core/
│   ├── Application.cpp/h       App lifecycle management
│   ├── AppTrackingService.cpp/h Polling loop & session management
│   ├── Database.cpp/h          SQLite operations
│   ├── Session.cpp/h           Session data model
│   └── TrackedApp.cpp/h        Tracked app data model
├── platform/
│   ├── AbstractProcessMonitor.h Interface for process detection
│   ├── WindowsProcessMonitor.*  Win32 Toolhelp32 implementation
│   └── LinuxProcessMonitor.*    /proc filesystem implementation
├── services/
│   └── DataPortService.cpp/h   CSV/JSON export & import
├── ui/
│   ├── MainWindow.cpp/h        Main dashboard window
│   ├── DashboardWidget.cpp/h   Stat cards grid
│   ├── AppListWidget.cpp/h     Searchable app sidebar
│   ├── SessionHistoryWidget.*   Per-app session table
│   ├── AddAppDialog.cpp/h      Add app from processes or browse
│   ├── AppDelegate.cpp/h       Custom list item rendering
│   ├── MacTitleBar.cpp/h       macOS-style traffic light title bar
│   ├── SettingsDialog.cpp/h    Settings with 4 pages
│   ├── SystemTrayManager.cpp/h Tray icon & context menu
│   ├── AnimationHelper.h       Fade/slide animations
│   └── themes/
│       └── ThemeManager.cpp/h  9 built-in themes
├── utils/
│   ├── Constants.h             Version, app name, defaults
│   └── TimeFormatter.cpp/h    Duration formatting
resources/
└── resources.qrc               Icons and stylesheets
```

---

## License

MIT — see [LICENSE](LICENSE) for details.
