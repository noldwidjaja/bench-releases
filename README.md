# Bench

A workstation for your Mac: your shells, your todos and every Claude Code
session, laid out on one bench. It lives in the menu bar, runs its own
terminal, pulls your GitHub issues and ClickUp tasks into a backlog, and
tells you at a glance which session needs you.

This repository holds Bench's builds. **[Download Bench.dmg](https://github.com/noldwidjaja/bench-releases/releases/latest/download/Bench.dmg)**
— always the latest version.

## Install

You need a Mac with **Apple silicon** (M1 or later) on **macOS 14 Sonoma or
later**.

1. **Download** [Bench.dmg](https://github.com/noldwidjaja/bench-releases/releases/latest/download/Bench.dmg).
2. **Open it**, and drag **Bench** onto the **Applications** folder beside it.
   Bench only runs from Applications; opened anywhere else it says so and
   quits. Eject the disk image afterwards.
3. **Let macOS open it.** Bench isn't signed with an Apple Developer ID, so
   the first time, macOS blocks it with *"Apple could not verify 'Bench' is
   free of malware…"*. Either way below works, and it's needed only once —
   updates arrive already trusted.

   **In Terminal** — quickest:

   ```sh
   xattr -dr com.apple.quarantine /Applications/Bench.app
   open /Applications/Bench.app
   ```

   `xattr -dr` removes the quarantine flag macOS puts on everything
   downloaded, from the app and everything inside it; without that flag,
   Gatekeeper doesn't stop it. Check it took with
   `xattr -l /Applications/Bench.app` — `com.apple.quarantine` should be
   gone from the list.

   **In System Settings:**
   1. Open Bench from Applications, and click **Done** when it's blocked.
   2. Open **System Settings → Privacy & Security**.
   3. Scroll down to the line about Bench being blocked, and click **Open Anyway**.
   4. Enter your password, and click **Open** when macOS asks once more.

4. **Go through the setup.** Bench opens a short walkthrough — about two
   minutes, and every step can be skipped and changed later in Settings:
   - **Look** — a theme.
   - **Projects** — the folders you work in, found in `~/dev` and the usual
     places, and the command that starts Claude in each.
   - **GitHub** — needs the [`gh` CLI](https://cli.github.com) signed in;
     the step installs it (`brew install gh`) and signs you in, in a tab.
     Then what each repo pulls into the backlog.
   - **Calendar**, **Claude Code**, **Shortcuts** and **Phone** — each says
     what it's for before it asks for anything.
5. **Allow what macOS asks for, as it comes up.** Nothing is asked up front:

   | Permission | Asked when | Used for |
   |---|---|---|
   | Notifications | the first notification | a tab that needs you, reminders, updates |
   | Calendars | you turn the calendar on | today's events beside your todos |
   | Accessibility | you attach a window to a todo | bringing other apps' windows back |
   | Microphone | something in a tab listens — Claude Code's voice mode | the program in the tab, not Bench itself |

6. **Optional, but worth it** — in Bench's **Settings**:
   - **Claude Code → Install in Every Claude Config Folder**: todos follow
     Claude sessions run outside Bench too.
   - **Claude Code → Teach Claude to Use bench**: in a Bench tab, Claude
     starts servers and watchers in tabs of their own (`bench open`), where
     you can see them, instead of in its own background shell.
   - The sidebar's **Show Claude's 5h limits**: each Claude account's 5-hour
     and 7-day windows as HP bars.
   - **Connectors → ClickUp**: your ClickUp tasks in the backlog.
   - **Phone**: Bench on your phone, over Tailscale.
   - **System Settings → General → Login Items**: add Bench to have it start
     with your Mac.

## Updates

Bench updates itself: it checks a minute after it starts and every six
hours, downloads a new version quietly, and installs it the next time it
quits — or now, with **Restart to Update** in the **Bench** menu or the
notification. **Bench → Check for Updates…** looks right away. Your tabs come
back after an update.

An update is only installed if it's signed by the same certificate as the
copy you have, and matches the checksum in
[`latest.json`](https://github.com/noldwidjaja/bench-releases/releases/latest/download/latest.json).

## Uninstall

1. Take Bench out of Claude Code first, or Claude Code goes on calling an app
   that's gone:
   - **Settings → Claude Code → Remove**, beside *Claude uses bench*, takes out
     the skill, the guard hook and the `CLAUDE.md` block.
   - The rest is by hand, in each Claude config folder's `settings.json`
     (`~/.claude/settings.json`, and `~/.claude-work/settings.json` or the
     like for a second account): delete the `hooks` entries whose command
     mentions `Bench.app`, and if `statusLine` starts with
     `'/Applications/Bench.app/…' --statusline`, put back what follows its
     `--` (or delete `statusLine` if nothing does).
2. Quit Bench, and drag it from Applications to the Bin.
3. Delete `~/Library/Application Support/Bench` for its data — todos,
   projects, settings, saved tabs:

   ```sh
   rm -rf ~/Library/Application\ Support/Bench
   ```