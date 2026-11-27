# Changelog (Open 5250)

> 日本語版は [`CHANGELOG_ja.md`](CHANGELOG_ja.md) を参照してください。 (Japanese version: see [`CHANGELOG_ja.md`](CHANGELOG_ja.md).)

Changes to Open 5250, most recent first.

Time-limited beta release. **Valid until 2027-01-31.** The source code is not published. Verified in a Japanese environment only (primary language 2962, QCCSID 65535).

---

## 2026-11-26 (v0.1.19-beta)

### Added
- **Colors of the 5250 screen** — The window opened from the "Colors" icon on the toolbar sets the colors of the same elements as the "Colors" of IBM i Access Client Solutions (the 7 field colors, the 4 OIA indicators and the OIA background, the screen background, the active field, the crosshair ruler and the column separator). The cursor inverts the colors under it, as in IBM i Access Client Solutions. IBM i Access Client Solutions color settings (*.col) can be imported and exported from the same window. "Save" records them in the preset (quick connect always starts with the defaults).
- **Aspect ratio of the 5250 screen** — Drag the handle at the bottom-right of the 5250 screen to change the aspect ratio; double-click it to restore the default. The number of columns and rows and the column positions of characters do not change (only the column width and row height change). "Save" records it in the preset.
- **Crosshair ruler** — A toolbar button shows or hides a horizontal line on the cursor row and a vertical line on the cursor column (hidden by default). The line color can be set in the "Colors" window, and the crosshair ruler color in *.col files is read and written.
- **Cursor blink and shape** — The cursor icon to the right of the mouse wheel button on the toolbar opens a window to choose whether the cursor blinks (on/off), the blink speed (a slider) and its shape (block/underline). The default is a blinking block. It is saved the same way as the shift character display.
- **Dedicated release time** — The time after the tab using a dedicated session is closed until other tabs can open it can be set in the admin screen settings (0–60 minutes, default 5; previously fixed at about 5 seconds).
- **User-defined character list** — Copy is now an icon only, and the explanatory text was removed. The list is shown in the display language only (Japanese or English). Hovering over a glyph enlarges it.
- **Manual for using the 5250 screen is included** — The IBM 5250 MCP User Guide (PDF, Japanese and English) is now included. Behavior and restrictions that differ from IBM 5250 MCP are listed in section 4.7 of the manual.
- **IBM i user-defined character management** — "Get user-defined characters" on the administration screen gets the UDC glyphs (QIGC3232 and QIGC2424) of this partition over FTP, and UDCs on the 5250 screen are drawn with those glyphs (32×32 first; a UDC without a glyph is drawn as a dotted box with a diagonal line). "Show user-defined characters" opens a list (headed by the system name and host; EBCDIC, PC (Shift-JIS) and Unicode codes and glyphs) in a new tab, and the copy icon lets you paste a UDC into the 5250 screen. See manual section 3.10.
- **The "Status" column of the session list shows the user, function and status of the signed-on job** — In the same form as WRKACTJOB (e.g. GURICAT / CMD-WRKACTJOB / DSPW). Hover over it to see the full text.
- **English guide** — When the display language is English, "Help" opens the English guide.

### Changed
- **Toolbar toggles are saved in the browser you use** — Input field highlighting, column separators, shift character display, blinking, crosshair ruler, cursor, toolbar hiding and the display language are saved in the browser (a cookie) and start the same whatever the port number (previously saved in the preset, and quick connect always started with the defaults). The font is still saved in the preset.
- **ILE API waits for the IBM i response after sending a key** — I5250KEY and I5250TXT (and the RPG procedures) now wait up to 30 seconds for the IBM i response after a key that gets a response (ENTER, F keys and so on). I5250SCR called afterwards reads the next screen directly (previously the screen had to be read every second until it changed). The SMPSGN sample in manual section 5.12 was simplified accordingly.
- **Initial display language and quick connect code page follow the primary language** — When the primary language of the IBM i is Japanese (2962 or 2930), the sign-in page, administration page and 5250 screen are shown in Japanese; otherwise in English (the 5250 screen used to follow the browser language when not Japanese). The default host code page for quick connect is 5026 for Japanese and 037 otherwise.
- **Buttons on the administration screen** — Settings (gear) and "Get" / "Show" of IBM i user-defined character management now have an icon, a label and a border. Icon buttons show a tooltip on hover.
- **The empty box below the 5250 screen is no longer shown** — It showed the communication phase for a moment only, so usually only an empty box was visible. The 5250 screen gets that space.
- **Section colors and the place of user-defined characters on the administration screen** — "Active sessions" has a light blue background and "Start a new session" a light green one, to tell them apart. "IBM i user-defined character management" (Get / Show) moved into the Settings window (the gear).
- **Code page and screen size of active sessions** — Hovering over the preset name in the list shows the host code page and screen size of that session (also for quick connect).
- **Layout of the administration screen** — "Start" is placed next to the "Start a new session" heading, and quick connect (the default) or a preset is chosen with radio buttons. The preset list and the AUTOTYP script list are folded. The settings moved to a window opened from the gear at the right end of the "Active sessions" heading. The mode of a shared session is shown as "Shared (n)".

### Fixed
- **A tab of a previous session no longer takes the next session** — A tab left open from a session that used the same port could reconnect automatically to the next session started on that port by someone else and become its owner. Each session now gets a new internal identifier when it starts, so old tabs cannot connect (the identifier is not shown on screen).
- **Pressing Help after a host error (such as "Function key not allowed") did not show the message help** — As in IBM i Access Client Solutions, the IBM i "Additional Message Information" is now shown (Help is sent as Help in the error state, as the 5494 specifies). Reset after returning from the help restores the original last line.
- **The glyphs of GUI elements were changed to be close to those of ACS** — Window borders, scroll bars, selection indicators and so on. The menu bar separator is a double line, and the top corners of a pull-down have the same shape as in IBM i Access Client Solutions.
- **A session that never received the sign-on screen was shown as "Signed on"** — When the connection is made but the sign-on screen does not arrive (the IBM i subsystem is not active, an English session connected to a Japanese subsystem, and so on), the status is now "Waiting for sign-on". A lost connection is now shown as "Disconnected" instead of "Not connected".
- **Ending a "Waiting for sign-on" session with "Wait" (*CNTRLD) never finished** — There is no job to sign off, so it is now disconnected without waiting (ENDI5250 DELAY(*NOLIMIT) and STRI5250 RESTART(*YES) used to hang).
- **"Immediate" (*IMMED) was not accepted for a session already ending with "Wait" (*CNTRLD)** — As with ENDJOB, a later "Immediate" now takes priority.
- **In an English (037 etc.) session, programs that read the screen (QsnRtvScr etc.) failed with CPFA306** — The query reply now reports the ideographic (kanji) capability only for the Japanese terminal (5555).
- **A paste that entered no characters (the field did not accept the first character) still marked the field as modified** — The field content did not change, but the field was sent to the host as modified with the next AID key, and a following Erase Input cleared the value the host had displayed. As when a typed key is rejected, the field is no longer marked modified (5494: pasting is the same as typing the keys).
- **Copying a multi-row selection copied nothing when a later row started on the right half of a double-byte character** — As in IBM i Access Client Solutions, nothing is copied only when the first row starts on a right half; on later rows that character is dropped and the rest is copied.
- **Backspace right after an SI (shift-in) stopped on the SI column** — As in IBM i Access Client Solutions, the cursor skips the SI and moves to the start of the preceding double-byte character.
- **Paste while input-inhibited (X SYSTEM) was written to the screen immediately** — As in IBM i Access Client Solutions, it is discarded (typed characters are still kept as type-ahead).
- **Arrow, Delete, Backspace and End (Erase EOF) keys pressed while input-inhibited had no effect after the unlock** — Like typed characters, they are now kept as type-ahead and applied in order after the unlock (as in IBM i Access Client Solutions).
- **A right click (also Shift + right click and Shift + middle click) moved the cursor to the clicked position** — As in IBM i Access Client Solutions, it does nothing unless the host defines the mouse button.
- **A middle click only moved the cursor** — As in IBM i Access Client Solutions, it moves the cursor and then sends Roll Up (next page), unless the host defines the button.
- **In an English (037 etc.) session, confirming kanji with the IME was rejected ("Field data must be alphanumeric")** — As in IBM i Access Client Solutions, characters not in the code page are written as "?" one column each (same as paste).
- **The slider position and size of short scroll bars differed from IBM i Access Client Solutions** — Now drawn the same way as the 27 cases measured in IBM i Access Client Solutions.
- **Scroll bar clicks sent different data from IBM i Access Client Solutions** — The cursor is no longer moved (only when the definition requests it), the scroll bar column is sent correctly, an arrow sends nothing when the slider is already at that end, and a click in the shaft sends the scroll bar size as the increment (5494 specifies 0; this follows IBM i Access Client Solutions as an exception).
- **Message when the file of a FILE:// URL hotspot does not exist** — As in IBM i Access Client Solutions, it is now "Path not found: FILE://…" (the URL as shown on the screen; previously "File not found: /…", or only an English reason when the host is not this partition or the share does not exist).
- **Window borders had no shadow** — As in IBM i Access Client Solutions, the right and bottom borders have a shadow.
- **The second and later push buttons placed side by side in one field were shifted one column to the left** — They now use the same spacing as IBM i Access Client Solutions (button frame width plus the gap between buttons).

## 2026-11-24 (v0.1.18-beta)

### Added
- **Per-session inactivity timeout and post-sign-on command when starting a session with an HTTP request** — `inactivity_timeout` (0 to 300 minutes, 0 = none) and `signon_command` (1 to 255 characters) of `POST /api/sessions`. The session status now has `last_activity_at` (time of the last exchange) and `inactivity_timeout` (for integration with other systems; User and Administration Guide section 4.6).
- **Sessions per process** — Added to "Settings" on the administration page (1 / 10 / unlimited, default 10). One server process handles several sessions, and a new session goes to the process with the fewest sessions. Starting 30 sessions went from 41.5 to 6.9 seconds and memory from 1,976 MB to 134 MB (both when put in one process). The risks (an abnormal end or forced end of a process ends all of its sessions; heavy processing makes other sessions wait) are described in the administration page help and manual section 3.7. A session's screen cannot see or operate other sessions of the same process.

### Changed
- **The browser tab icon is now a terminal** — The 5250 screen shows a terminal icon with a blue frame and green text, and the administration page one with a blue frame and amber text (previously the default globe icon).
- **Messages of the commands and the ILE API are now CPF9897 (previously CPF9898)** — The text of CPF9898 is "&1。" and adds a period, which was shown after a blank. Programs that use `MONMSG MSGID(CPF9898)` must be changed to `CPF9897`.
- **Message texts were cleaned up** — Blanks between double-byte and single-byte characters were removed (EBCDIC inserts shift characters there, so the blanks were doubled), and texts ending in a double-byte character end with a period. Messages returned from the server are in uppercase (lowercase letters turn into katakana in 5026 jobs). Command prompts and object texts follow the same rules.
- **The license is now an expiry date only** — Previously it started only on an IBM i with the licensed machine serial number and partition. It now starts on any IBM i until the expiry date (2027-01-31).
- **Published** — Distributed on GitHub (GuriCat/Open5250).
- **Library I5250 is distributed as a save file** — The ILE API and CL commands are restored from `I5250.SAVF` in the package (IBM i V7R4M0 or later; manual section 2.2).
- **The administration page job runs at run priority 20** — The administration page job runs `CHGJOB RUNPTY(20)` at startup, and session processes inherit it (previously the QSYSWRK default of 50). Typed input is less likely to be delayed even when other batch jobs keep using the CPU.
- **Session logs are deleted after 7 days** — Dated session logs in `log/` (`*_YYYYMMDD-HHMMSS.log`) older than 7 days are deleted by the administration page at startup and once a day.
- **Host names in URL hotspot file:// and in STRPCCMD `\\host\…` are checked by name resolution** — Previously they were only compared as strings with the names of this partition (localhost, system name, host name, own IP), so aliases in the host table or DNS (for example LOOPBACK) were treated as "not this partition" and not opened. They are now resolved to an IP address and opened if it is this partition's IP. Because name resolution on IBM i can take time (8 seconds per name on GURI), resolution starts as soon as a file:// appears on the screen, and the result is remembered for 10 minutes.
- **A session started on the same port right after a session ended could not be opened from the 5250 screen** — The step that closes the ended session's 5250 screen port 0.5 seconds later also closed the 5250 screen of a new session started on the same port in the meantime. A port used by a new session is no longer closed, and when the specified port is in use the number no longer shifts to another one (that made the number in the administration page list differ from the actual one).
- **The administration page heading and browser tab name are now "Open 5250"** — The heading is "Open 5250 セッション管理" ("Open 5250 Session Manager" in English) and the tab name is "Open 5250" (previously "IBM 5250 セッション管理" and "IBM 5250").

### Fixed
- **Viewer selection could not select a single column** — Mouse drag and Shift+Arrow can now select one column (including half of a double-byte character). Shift+Arrow moves the corner of the selection without moving the cursor (as in IBM i Access Client Solutions).
- **Copy included a whole double-byte character when only half of it was in the selection** — As in IBM i Access Client Solutions, that character is not copied. When the left edge of the selection is the right half of a double-byte character, nothing is copied. Shift-character columns become blanks.
- **Grid lines of the screen behind a window showed through inside the window** — As 5494 specifies (creating a window clears its area), grid lines inside the window area are cleared.
- **Double-byte line-drawing characters (such as ━) were drawn in a different font from other characters and did not join** — They are now drawn in the same font as the rest of the screen.
- **Clicking the right half of a double-byte character put the cursor in the middle of the character** — As in IBM i Access Client Solutions, it goes to the start of the character.
- **Paste put single-byte characters into J and G fields (double-byte only) and mixed single- and double-byte characters in E fields** — As 5494 specifies (paste is checked like typing), each character is checked and the paste stops at the first error. As in IBM i Access Client Solutions, no message is shown.
- **Paste in insert mode overwrote, and the cursor moved after a paste** — In insert mode the following characters are now shifted right. After a paste the cursor stays at the paste start in both modes (as in IBM i Access Client Solutions).
- **Paste stopped silently when there was no room to insert, did nothing at a protected position, and wrote nothing (not even "12") when "12A" was pasted into a numeric field** — It now matches measurements in IBM i Access Client Solutions. When there is no room to insert, it inserts what fits, shows "No room to insert data", and leaves the cursor where it stopped. At a protected position, it pastes into the next input field (the cursor does not move; 5494 specifies an operator error, so this is an exception that follows IBM i Access Client Solutions). In fields such as numeric-only fields, it writes the characters before the first one that is not accepted (no message).
- **Pasting kanji or half-width katakana in an English (037 or other single-byte) session garbled the screen** — they were written as double-byte characters. As in IBM i Access Client Solutions, characters not in the code page are now written as "?" in one column (the host receives the same characters).
- **The notice shown on the 5250 screen when a PC command (STRPCCMD) ran is no longer displayed** — The result of the run is not shown on the screen.
- **Mixing half-width letters and half-width katakana in an E-type (DBCS-either) field was rejected** — In 5026 (katakana) sessions, the input check of MCP send_text and AUTOTYP counted half-width katakana as double-byte and rejected "Aｱ" with "Field data must be alphanumeric". It now uses the code page table, like the viewer.
- **In an error line inside a window, the error text overlapped the area left of the window when that area had half-width katakana** — half-width katakana were counted as two columns.
- **Half-width katakana confirmed with the IME was rejected with "Field data must be alphanumeric" in fields that do not accept double-byte characters** — In 5026 (katakana) sessions, characters confirmed with the IME are now classified as single- or double-byte by the code page table (typing one character at a time was fixed on 2026-11-10).
- **When the same PC command (STRPCCMD) string was run twice in a row, the second one was not run and the IBM i program stopped waiting for the reply** — The second one is now run too.
- **An English sign-on screen (Sign On) was not recognized as a sign-on screen** — With an interactive subsystem whose primary language is English, in cp1399 and similar sessions, the status display on the administration page and the controlled end of ENDI5250 (waiting for the sign-on screen) did not finish. The screen bytes are now also read as 037 to decide.
- **The share name of files in shared folders (URL hotspot `file://`, PC command `\\host\…`) is now converted to an IFS path using the NetServer file share definitions** — Previously the share name was only treated as a directory name directly under the root, so files in shares whose name differs from the path (for example share LMS → /tmp/PCDOC) could not be opened. Share names not in the definitions (QSYS2.SERVER_SHARE_INFO) are not opened.
- **The description of PC commands (STRPCCMD) was expanded** — How the target is decided (the target of START; URLs and shared-folder files decided by the leading characters) and the restrictions are described in sections 4.5.1 and 3.8.4 of the User and Administration Guide.
- **Starting sessions at the same time assigned the same port** — When several sessions were started almost simultaneously from the administration page, the same port was assigned and the same session could be returned to two people.

---

## 2026-11-10 (v0.1.17-beta)

### Changed
- **Names with "N" were unified to "I"** — Mixed names were aligned to I5250.

  | Old | New |
  |---|---|
  | Library `NODE5250` | `I5250` |
  | Install dir `/node5250` | `/i5250` |
  | Commands `STRN5250` / `ENDN5250` | `STRI5250` / `ENDI5250` |
  | Administration page job `N5250ADM` | `I5250ADM` |
  | Help `N5250H`, data area `N5250DIR` | `I5250AH`, `I5250DIR` |
  | Log `log/n5250cmd.log` | `log/i5250cmd.log` |

  RPG declarations and binding become `/COPY I5250/QRPGLESRC,I5250API_H` and `H BNDDIR('I5250/I5250')`.
- **The connection target of a preset can be specified** — On the connection configuration screen you can enter a host name (or IP address) and port (blank = this partition 127.0.0.1:23). A "Target" column was added to the list. Quick connect connects to this partition.
- **The default of shared/dedicated is now dedicated** — For the preset rows on the administration page and the default of MODE of `I5250STR` (*DEDICATED). Shared is a special use where several people view and operate the same session at the same time.
- **Quick connect no longer opens the screen automatically** — As with presets, "Start" starts the session and "Open" in the list shows the screen.
- **Dedicated is the default everywhere** — The administration page API, `start-session.sh`, and ILE `I5250_Start` (when Mode is blank) are now dedicated too (previously shared). Shared is a special use where several people view and operate the same session at the same time.
- **The product name shown on 5250 screens is now "OPEN 5250"** — The prompt titles and help of STRI5250 and ENDI5250, the end messages, and the object descriptions of library I5250. It is in uppercase so that it is not garbled on Katakana code pages. The footer of the administration page is also "Open 5250".
- **Added the manual `docs/Open5250-Guide.pdf` (User and Administration Guide)** — Operating procedures, CL command syntax, ILE RPG procedure specifications, with pictures.
- **Presets are now one file per preset** — `/i5250/presets/<name>.i5250` ("key=value" per line). The settings screen on the administration page is unchanged.
- **5250 screen display settings (input field highlighting, column separators, shift character display, blinking, font, toolbar hiding) start from the defaults each time for quick connect** — Previously they were saved in the browser, but the storage was per session port and ports are reused, so a session started with the state of the previous user of that port. For sessions started from a preset, the switched state is saved in that preset's file and used the next time that preset is started. Only the display language is saved in the browser.
- **"End" in the list was merged into a "Disconnect" button** — Choose "Immediate" (default) or "Wait" (seconds) on its right.
- **"Saved" and "Deleted" on the administration page are shown in green** — Previously they were red like errors.
- **The window mode switch and microphone (voice input) buttons were removed from the 5250 screen toolbar** — They did nothing in Open 5250.
- **The person icon on the 5250 screen toolbar shows whether the session is dedicated (one person) or shared (two people)** — It has no button background and does not toggle (previously it always showed shared and did nothing when pressed).

### Added
- **Inactivity timeout** — When 5 to 300 minutes is set in "Settings" on the administration page, a signed-on session with no exchange with the IBM i for that time is ended by the same procedure as immediate. Sessions on the sign-on screen and sessions running a command (keyboard locked) are excluded.
- **"Device name" column in the list** — Shows the display device assigned by the IBM i (for example QPADEV0003).
- **"Copy link" next to "Open" in the list**, and **a choice of how to open the screen (new tab / new window)**.
- **Elapsed seconds are shown while a session is starting**.
- **Help** — "Help" at the top right of the administration page and ℹ️ on the 5250 screen open the Open 5250 guide. A chapter on the ILE API (command syntax, RPG procedures, key names, samples) was added to the guide.
- **URL hotspots** (same as ACS) — URLs shown on the screen (http / https / www / mailto / ftp / file / gopher) are underlined and open in a new window when clicked. URLs in input fields are excluded. `file://` (for example `FILE://GURI/IMG/658.JPG`) is read from the IFS and returned by Open 5250, only when it is shown on the screen. Because the share name is treated as a directory name directly under the IFS root, only shares whose share name and directory name are the same can be opened (Open 5250 guide chapter 2).
- **Opening files with PC commands (STRPCCMD)** — Files that an IBM i program asks the PC to open with STRPCCMD (PDF, text, images such as `START \\GURI\TMP\…\x.PDF`) are opened in a new window of the 5250 screen's browser. The command itself is not run (starting Excel and the like is not available). Previously all STRPCCMD were ignored.
- **Auto-refresh indicator on the administration page** — "Refresh" and a round icon that cycles every 5 seconds are shown next to the "Sessions" heading. Refreshing stops and the icon dims while an input field or selection is being operated.

### Fixed
- **Pressing ℹ️ on the 5250 screen did not show the help**.
- **"mcp" appeared in the 5250 screen tab name** — It now shows the device name, target and port.
- **Selection fields (radio buttons, check boxes, push buttons) are sent as described in the IBM manual** — Only changed selection fields, one byte per choice for check boxes, in position order. Push button clicks are also sent with which button was clicked. The display of check boxes in windows (□/☒), red blinking (toggled with the light bulb icon), and resending of input fields outside a window were also fixed. The keyboard input target, mouse and wheel settings on the toolbar were moved to the second row.
- **Choosing a menu bar or pull-down choice with a click or space was not passed to the IBM i** — The Enter key and the chosen choice are now sent when it is chosen (auto-enter selection fields).
- **Double-click did not send the Enter key** — A double-click with no assignment on the screen side becomes the Enter key. A second click or double-click at the position where the cursor was moved sometimes did not work; this was also fixed.
- **"mcp" appeared in the 5250 screen address and tab title** — The session ID is no longer added to the address (also for windows opened from image hotspots). The title also switches immediately when the language is switched.
- **After closing a screen overlaid on another (such as help), the input fields and push buttons of the original screen could not be used** — After the screen was restored (RESTORE SCREEN), the field list was lost and push buttons, choices and scroll bars became plain input fields.
- **Layout of the administration page** — Split into "Active sessions" (list and settings) and "Start a new session" (quick connect and presets), with the preset list and AUTOTYP script list in the preset frame. AUTOTYP scripts can be edited and deleted from the list.
- **With 5026 Katakana (old 290), lowercase letters sent by the host were displayed as blanks** — Display now uses the current 290 table, which has lowercase letters (same as other IBM emulators). Input is old 290 as before (lowercase becomes uppercase).
- **Half-width Katakana could not be entered with 5026** — Deciding whether an input character is single-byte or double-byte now uses the code page table.
- **The ATTN key disconnected the session (CPF5140)** — The response to QUERY STATION STATE now follows the 5494 specification.

## 2026-11-09 (v0.1.16-beta)

### Fixed
- **"Started on port n" remained on the administration page after the session ended** — It is now cleared when the session disappears from the list or when an end operation is done.
- **Nothing was shown on the administration page when communication failed** — An error is now shown when it cannot connect or there is no valid response. It is also shown during auto-refresh of the list and cleared when communication returns.
- **In a dedicated session, if the PC in use crashed or the line stayed disconnected, other tabs could not use it** — The server asks the tab in use every 15 seconds and releases it after about 1 minute without a response (reloads and short line interruptions do not release it).
- **Ending a session with "wait" (*CNTRLD) kept waiting for the full wait time even after the connection to the IBM i was lost** — It now ends when the connection is lost.

### Added
- **"Quick connect" on the administration page** — Under the "Presets" row, choose a code page and screen size and press "Connect" to start a new session without creating a preset and open the 5250 screen in another browser tab. The defaults follow IBM i Access Client Solutions (930 = 5026 Japanese Katakana, 24x80). Shared/dedicated is also chosen in this row, and the default is dedicated. The target is that partition (127.0.0.1:23), as with presets.
- **CL commands `STRN5250` / `ENDN5250` (library NODE5250)** — `STRN5250` starts the administration page in batch job N5250ADM (default job queue QSYS/QSYSNOMAX). `RESTART(*YES)` ends all 5250 sessions and the administration page and then starts again. `ENDN5250` ends all 5250 sessions and the administration page. The values and defaults of `OPTION(*CNTRLD|*IMMED)` and `DELAY(1-99999|*NOLIMIT)` follow ENDSBS. Only users with user class *SECOFR can run them. Each parameter is described in the F1 help.
- **Help for the 8 ILE API commands (I5250STR to I5250ATP)** — Press F1 at the prompt (F4) to show descriptions of the command and each parameter.

### Changed
- **When a dedicated session is opened in a second tab, that tab is closed after a warning** — Previously it remained as a blank screen. Because of browser restrictions, only tabs opened with "Open" or "Connect" on the administration page (and their duplicates) can be closed. Tabs opened by entering the URL in the address bar and the like remain blank as before.
- **Tabs duplicated with the browser's "Duplicate tab" are treated as separate tabs** — Previously a duplicated tab could also use a dedicated session.
- **"共有" was changed to "共用"** — To match the HMC 5250 console wording (共用 / 専用, shared / dedicated), the administration page, documents and command displays use "共用". The values (`*SHARED` / `shared`) are unchanged.
- **Added the manual "Open 5250 guide"** (`docs/open5250-guide.html`) — Starting sessions (presets, quick connect), operation in the browser, multiple sessions, the behavior of shared and dedicated, starting and ending the administration page (STRN5250 / ENDN5250), and the list of ILE API commands. It is a separate file from the IBM 5250 MCP manual.
