<div align="center">

<img src="assets/banner.svg" alt="perm — Permission and Metadata Analyzer, a Bash CLI for Linux" width="720"/>

<p align="center">
  <img src="assets/typing.svg" alt="What permissions does this file have? What changed since last time? Why can't user X access this file?" width="480"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white" alt="Bash"/>
  <img src="https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black" alt="Linux"/>
  <img src="https://img.shields.io/badge/version-3.4-58A6FF?style=flat-square" alt="Version 3.4"/>
  <img src="https://img.shields.io/badge/dependencies-none-3FB950?style=flat-square" alt="No dependencies"/>
</p>

<p align="center">
A Bash CLI that <b>explains Linux file permissions</b>, <b>detects permission drift</b>,<br/>
and tells you <b>why</b> a user can or cannot access a file.<br/>
One script. No dependencies beyond standard GNU/Linux tools. Targets Ubuntu/Linux.
</p>

</div>

<p align="center"><img src="assets/divider.svg" alt="" width="720"/></p>

<p align="center"><img src="assets/header-questions.svg" alt="What it answers" width="520"/></p>

<p align="center">
  <img src="assets/questions.svg" alt="perm answers three questions: what permissions a file has, what changed since last time, and why a user cannot access a file" width="780"/>
</p>

<p align="center"><img src="assets/header-features.svg" alt="Features" width="520"/></p>

<p align="center">
  <img src="assets/features.svg" alt="Features: analysis and explanation, comparison, drift detection with baselines, and effective access analysis with --why" width="780"/>
</p>

<p align="center"><img src="assets/header-install.svg" alt="Installation" width="520"/></p>

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

<p align="center"><img src="assets/header-usage.svg" alt="Usage" width="520"/></p>

<p align="center">
  <img src="assets/usage-synopsis.svg" alt="Command synopsis of perm: analyze, quiet, compare, explain, baseline, check, scan, list, remove and why" width="720"/>
</p>

<details>
<summary>Copy the synopsis as plain text</summary>

```text
perm [OPTIONS] PATH
perm --quiet PATH
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

</details>

<img src="assets/label-analyze.svg" alt="Analyze and explain" width="100%"/>

```bash
perm /etc/passwd
perm --security /etc/passwd
perm --report /etc/passwd
perm --quiet /etc/passwd          # permission string only
perm --json /etc/passwd

perm --explain 755
perm --explain 6755
```

<img src="assets/label-compare.svg" alt="Compare two files" width="100%"/>

```bash
perm --compare file1 file2
perm --compare --permissions file1 file2
perm --compare --special file1 file2
perm --compare --diff file1 file2   # only the real differences
```

<img src="assets/label-drift.svg" alt="Detect drift with baselines" width="100%"/>

```bash
perm --baseline /usr/bin/sudo
perm --check /usr/bin/sudo
perm --baseline --strict /usr/bin/sudo     # also track content (SHA-256)

perm --scan --recursive /etc
perm --scan --recursive --check --changes-only /etc

perm --list
perm --remove /usr/bin/sudo
```

<img src="assets/label-why.svg" alt="Why can (or can't) a user access a file?" width="100%"/>

```bash
perm --why bob read /srv/app/data.txt
perm --why www-data execute /usr/local/bin/tool
perm --why alice delete /tmp/report.txt
perm --why 1001 w /var/www/index.html      # numeric UID and short action work too
```

<p align="center"><img src="assets/header-output.svg" alt="Sample Output" width="520"/></p>

<p align="center">
  <img src="assets/demo-explain.svg" alt="Terminal: perm --explain 6755 shows owner, group and others access plus enabled SUID and SGID bits" width="720"/>
</p>

<p align="center">
  <img src="assets/demo-why.svg" alt="Terminal: perm --why nobody delete shows a path walk and a DENIED result because the parent directory lacks write permission" width="720"/>
</p>

<p align="center">
  <img src="assets/demo-drift.svg" alt="Terminal: perm --scan --check lists NEW, OK, CHANGED and DELETED files and exits with code 1" width="720"/>
</p>

<p align="center"><img src="assets/header-exit.svg" alt="Exit Codes" width="520"/></p>

<p align="center">
  <img src="assets/exit-codes.svg" alt="Exit codes: 0 success or no change, 1 differences found or access denied, 2 error or invalid usage" width="780"/>
</p>

<p align="center">Set <code>NO_COLOR=1</code> to disable colored output. Colors are also off when output is not a terminal.</p>

<p align="center"><img src="assets/header-limits.svg" alt="Limitations" width="520"/></p>

- ACLs are detected (a warning is shown) but **not** evaluated
- SELinux/AppArmor, Linux capabilities and `chattr +i` are not considered
- `--strict` (content hash) works on regular files only and is not supported with `--scan`
- Baselines are stored in `${XDG_DATA_HOME:-~/.local/share}/perm/baselines/`
- `perm` is a lightweight metadata checker, **not** a replacement for AIDE, Tripwire or SELinux tooling

<p align="center"><img src="assets/header-author.svg" alt="Author" width="520"/></p>

<p align="center">Made by <a href="https://github.com/Kourosh-NF">Kourosh</a>, an IT student learning Linux, networking and infrastructure.</p>

<p align="center">
  <a href="mailto:kouroshnf87@gmail.com"><img src="assets/contact-email.svg" alt="Email: kouroshnf87@gmail.com" width="520"/></a>
</p>

<p align="center">
  <a href="https://t.me/albernfnx"><img src="assets/contact-telegram.svg" alt="Telegram: @albernfnx" width="520"/></a>
</p>

<p align="center">
  <a href="https://github.com/Kourosh-NF"><img src="assets/contact-github.svg" alt="GitHub: Kourosh-NF" width="520"/></a>
</p>

<p align="center">
  <img src="assets/footer.svg" alt="Thanks for visiting perm!" width="480"/>
</p>
