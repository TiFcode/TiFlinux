
# Ubuntu System Overview Script Documentation

## Purpose

Produces a system overview text file that highlights the **delta** from a stock Ubuntu installation.

## Usage

Copy the text content of "Delta from stock Ubuntu - paste into terminal.txt"
Paste directly into the terminal.

## Output

- The script is created and made executable by the provided setup snippet.
- Writes a report to `~/system-overview.txt` (overwrites existing file).
- The report contains sections for:
	- Manually installed packages
	- Extra APT repositories
	- Running services
	- Listening network ports
	- Non-system users
	- `/opt` contents
	- `/usr/local` binaries

## Permissions & Environment

- **No root privileges** are strictly required to run, but some commands (e.g., `ss -tulnp` or reading other users' processes) may be limited without `sudo`, so that section may report less information or print an error note.
- Assumes a **Debian/Ubuntu environment** with `apt-mark` available and `systemd` (`systemctl`). If `systemd` is absent, the running-services section will indicate "systemd not available".

## Report Sections & Behavior

- **Header:** Hostname, generation timestamp, and user who ran the script.
- **Manually installed packages:** Uses `apt-mark showmanual` to list packages the admin explicitly installed; the count is included.
- **Third-party / extra APT repositories:** Scans `/etc/apt/sources.list*` for `deb` lines and filters out official Ubuntu mirrors (`ubuntu.com`, `security.ubuntu.com`, `archive.ubuntu.com`). If none remain, prints a note.
- **Running services:** Lists systemd services in the running state (unit and active state). Falls back with a message if systemd is not present.
- **Listening network ports:** Lists listening sockets via `ss -tulnp`; may require `sudo` for PID/program fields and will print a failure note if `ss` is unavailable or permission denied.
- **Regular users (non-system):** Extracts passwd entries for UIDs >= 1000 and UID 0 (root), excluding `nobody`.
- **/opt contents:** Shows `/opt` directory listing (or an error note if unreadable).
- **/usr/local/bin and /usr/local/sbin:** Shows listings for locally installed executables; prints a note if empty/unreadable.
- **Closing note:** Indicates where the report was saved and how to read it.

## Failure Handling

The script redirects many command errors to `/dev/null` and prints brief explanatory messages in the report when a command fails or is not available.

## Security & Privacy Considerations

The report may include sensitive information such as service names, listening ports, local users, and package lists. **Treat the generated file as potentially sensitive and avoid sharing it publicly without review.**


