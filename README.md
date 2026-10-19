# Campus Event Booking

Class practical for Software Verification and Validation.

The website has **five bugs**. The tests that come with the project still pass: you should see **27 passed**. Your team picks one GitHub issue, shows the bug, writes a test that fails, fixes that bug, and opens a pull request.

The rules are in [`docs/app-spec.md`](docs/app-spec.md). If the website and that file disagree, the website is wrong.

Repository: <https://github.com/iftikhar69/svv-playwright-practical>

Windows 10 and 11 are the main instructions. Ubuntu is below them.

---

## 1. Windows

### Install once

1. Install **Python 3.12** from <https://www.python.org/downloads/windows/>. Tick **Add python.exe to PATH**. Do not use the newest button if it is Python 3.14.
2. Install **Git** from <https://git-scm.com/download/win>.
3. Make a free GitHub account.

Open **PowerShell** and run one line at a time:

```powershell
python --version
git --version
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

`python --version` should show Python 3.12. Use the email on your GitHub account.

### Get the code

On GitHub, open the class repository and click **Fork**, then **Create fork**. Click the green **Code** button and copy the HTTPS address of **your fork**.

```powershell
cd C:\src
git clone https://github.com/YOUR-USERNAME/svv-playwright-practical.git
cd svv-playwright-practical
py -3.12 -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m playwright install chromium
python -m pytest
```

If PowerShell blocks the activate script, run this once and then try again:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

If `py` is not found, use `python -m venv .venv` instead of the `py -3.12` line.

The last command must end with `27 passed`.

Every new PowerShell window:

```powershell
cd C:\src\svv-playwright-practical
.venv\Scripts\Activate.ps1
```

### Open the website

```powershell
python serve.py
```

In Chrome open <http://127.0.0.1:8000>. Press `Ctrl+C` to stop it.

### Fix your issue and send a pull request

Do this only after your team has claimed one issue.

```powershell
git switch -c issue-3-student-discount
```

Change `3` and `student-discount` to your issue. Add a test file in `tests/`, fix only that bug in `app/app.js`, then run:

```powershell
python -m pytest
```

When everything passes:

```powershell
git add .
git commit -m "Fix #3: student discount"
git push -u origin issue-3-student-discount
```

On GitHub, click **Compare & pull request**. Base repository is `iftikhar69/svv-playwright-practical`, base branch is `main`, and your branch is the one you just pushed. Fill in the form. Paste your test output. A screenshot from Chrome is useful. Click **Create pull request**.

---

## 2. Ubuntu

```bash
sudo apt update
sudo apt install -y python3 python3-venv python3-pip git
python3 --version
git --version
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Fork the repository on GitHub first, then:

```bash
cd ~
git clone https://github.com/YOUR-USERNAME/svv-playwright-practical.git
cd svv-playwright-practical
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m playwright install --with-deps chromium
python -m pytest
```

You should see `27 passed`.

Ubuntu 26.04 needs Playwright 1.64, which is already pinned in `requirements.txt`. Version 1.56 stops with `Playwright does not support chromium on ubuntu26.04-x64` and never downloads the browser. If you still see that error, run `git pull`, then run the two install commands again.

```bash
python serve.py
```

Open <http://127.0.0.1:8000> in Chrome.

New terminal:

```bash
cd ~/svv-playwright-practical
source .venv/bin/activate
```

Branch, test, and push:

```bash
git switch -c issue-3-student-discount
python -m pytest
git add .
git commit -m "Fix #3: student discount"
git push -u origin issue-3-student-discount
```

Open the pull request in Chrome the same way as on Windows.

---

## 3. What your team does

1. Comment on one issue: "we are taking this".
2. Follow section 1 or section 2.
3. Reproduce the bug in Chrome.
4. Add a test in `tests/`. Run it and see it fail.
5. Fix only that bug in `app/app.js`.
6. Run `python -m pytest`. Everything must pass.
7. Open a pull request. Do not merge it yourself.

Do not change a test so that the wrong result looks correct. A useful test fails before the fix and passes after it.

A small example:

```python
from playwright.sync_api import Page, expect
from tests.helpers import open_app

def test_student_price_for_hackathon(page: Page):
    open_app(page)
    page.get_by_test_id("event-select").select_option("hackathon")
    page.get_by_test_id("student-checkbox").check()
    expect(page.get_by_test_id("price-display")).to_have_text("$40.00")
```

Useful test ids: `name-input`, `email-input`, `event-select`, `student-checkbox`, `submit-button`, `price-display`, `form-error`, `name-error`, `email-error`, `confirmation`, `confirmation-name`, `confirmation-event`, `confirmation-email`, `confirmation-amount`, `booking-item`.

Event ids: `python-workshop`, `hackathon`, `robotics-night`.

---

## 4. If it fails

| What you see | What to do |
|---|---|
| `python` opens the Microsoft Store | Reinstall Python 3.12 and tick **Add python.exe to PATH**. Open a new PowerShell. |
| Running scripts is disabled | `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` |
| `No module named pytest` | Activate `.venv` again. The prompt must start with `(.venv)`. |
| Playwright cannot find the browser | Windows: `python -m playwright install chromium`. Ubuntu: `python -m playwright install --with-deps chromium` |
| `does not support chromium on ubuntu26.04` | Pull the latest `main` so `requirements.txt` says `playwright==1.64.0`, then run `python -m pip install -r requirements.txt` and `python -m playwright install --with-deps chromium` |
| Page does not load | Use <http://127.0.0.1:8000>, and leave `python serve.py` running. |
| `git push` is rejected | You cloned the class repository. Fork it, clone your fork, and push again. |
| Git asks for a password | In Chrome open <https://github.com/settings/tokens>, create a token, and paste the token. |
