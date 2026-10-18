# perm

**A Bash CLI that explains Linux file permissions, detects permission drift, and tells you *why* a user can or cannot access a file.**

![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)
![Version](https://img.shields.io/badge/version-3.4-58A6FF?style=flat-square)

`perm` answers three questions:

1. **What permissions does this file have?**
2. **What changed since last time?**
3. **Why can't user X access this file?**

It is a single Bash script with no dependencies beyond standard GNU/Linux tools. It targets Ubuntu/Linux.

---

## Features

**Analysis and explanation**
- Shows mode, owner, group and special bits (SUID, SGID, sticky) separately
- `--security` and `--report` give security-focused and structured reports
- `--explain 6755` turns an octal value into plain language
- `--json` output for scripting

**Comparison**
- `--compare` compares two files by permissions, ownership and special bits

**Drift detection (baselines)**
- `--baseline` saves the current state of a file, `--check` compares against it later
- `--strict` also records a SHA-256 hash of the file content
- `--scan` / `--scan --recursive` do the same for every file in a directory (symlinks are never followed, mount points are never crossed)
- Results: `[OK]` `[CHANGED]` `[NEW]` `[DELETED]` `[INVALID]` `[ERROR]` `[SKIP]`

**Effective access (`--why`)**
- Explains whether a user can `read`, `write`, `execute` or `delete` a path, step by step
- Follows kernel rules: `x` on every directory in the path, symlinks, first matching class only (owner, then group, then others), sticky bit, and `ro` / `noexec` mount options

---

## Installation

```bash
git clone https://github.com/Kourosh-NF/perm.git
cd perm
chmod +x perm
./perm --help
```

Optional: install it system-wide.

```bash
sudo install -m 755 perm /usr/local/bin/perm
```

---

## Usage

```text
perm [OPTIONS] PATH
perm --compare [OPTIONS] FILE1 FILE2
perm --explain VALUE
perm --baseline [--force] [--strict] PATH
perm --check [--json] PATH
perm --scan [--recursive] [--force] DIR
perm --scan [--recursive] --check [--json | --changes-only] DIR
perm --list [--json]
perm --remove PATH
perm --why [--json] USER ACTION PATH
```

### Examples

```bash
# Analyze a file
perm /etc/passwd
perm --security /etc/passwd
perm --report /etc/passwd

# Explain a numeric mode
perm --explain 755
perm --explain 6755

# Compare two files
perm --compare file1 file2
perm --compare --special file1 file2

# Detect changes with a baseline
perm --baseline /usr/bin/sudo
perm --check /usr/bin/sudo
perm --baseline --strict /usr/bin/sudo     # also track content (SHA-256)

# Scan a directory
perm --scan --recursive /etc
perm --scan --recursive --check --changes-only /etc

# Why can (or can't) a user access a file?
perm --why bob read /srv/app/data.txt
perm --why www-data execute /usr/local/bin/tool
perm --why alice delete /tmp/report.txt
```

### Sample output

`perm --explain 6755`

```text
Permission: 6755
Symbolic : -rwsr-sr-x

Owner:
  Read    : YES
  Write   : YES
  Execute : YES
...
Special Bits:
  SUID    : ENABLED
  SGID    : ENABLED
  Sticky  : disabled
```

`perm --why root read /etc/hostname`

```text
Path Walk (search permission x is needed on every directory)
  [OK]      dir    /
  [OK]      dir    /etc
  [OK]      target /etc/hostname

Result: ALLOWED
```

---

## Exit codes

| Code | Meaning |
|------|---------|
| `0`  | No differences / no change / success (`--why`: access allowed) |
| `1`  | Differences or change detected (`--why`: access denied) |
| `2`  | Error or invalid usage (including: no baseline exists) |

Set `NO_COLOR=1` to disable colored output.

---

## Limitations

- ACLs are detected (a warning is shown) but **not** evaluated
- SELinux/AppArmor, Linux capabilities and `chattr +i` are not considered
- `--strict` (content hash) works on regular files only and is not supported with `--scan`
- Baselines are stored in `${XDG_DATA_HOME:-~/.local/share}/perm/baselines/`
- `perm` is a lightweight metadata checker, **not** a replacement for AIDE, Tripwire or SELinux tooling

---

## Author

Made by [Kourosh](https://github.com/Kourosh-NF), a student learning Linux, networking and infrastructure.
Email: kouroshnf87@gmail.com · Telegram: [@albernfnx](https://t.me/albernfnx)
