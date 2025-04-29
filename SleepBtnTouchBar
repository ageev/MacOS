# How to Customize the Touch Bar with BetterTouchTool for Mac

## Overview
This guide demonstrates how to add custom buttons (including a **Sleep** button) and standard **F1–F12 function keys** to your Mac's Touch Bar using **BetterTouchTool** (BTT). Additionally, it covers how to automatically start BTT with macOS and remove the default BTT icon from the Touch Bar.

## 1. Install BetterTouchTool

1. Download BetterTouchTool from [https://folivora.ai](https://folivora.ai).
2. Install and grant the necessary accessibility and automation permissions.

## 2. Add Function Keys (F1–F12) to the Touch Bar

1. Open **BetterTouchTool**.
2. Navigate to **Touch Bar > Global**.
3. Add a new button for each function key (F1 through F12):
   - Click **"+ Add New Button"**.
   - Set **Name** (e.g., `F1`, `F2`, etc.).
   - Optionally, set an **Icon** or use the default text.
   - Under **Assigned Action**:
     - Action Type: **Send Shortcut to Specific App or System**.
     - Shortcut: Select the corresponding function key (F1, F2, etc.).
4. Repeat for all function keys (F1–F12).

## 3. Add a Sleep Button to the Touch Bar
1. Add another button for **Sleep**.
2. Set the **Name** to `Sleep` and an optional **Icon**.
3. Under **Assigned Action**, set the script to lock and sleep the Mac:
   ```bash
   /usr/bin/osascript -e 'tell application "System Events" to keystroke "q" using {control down, command down}'; sleep 1; /usr/bin/pmset sleepnow
