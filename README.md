# Kindle Sender

A simple command-line tool to convert a web page to a PDF and email it directly to your Kindle.

---

## 1. Prerequisites

This script requires the following tools to be installed on your macOS system:

- **Google Chrome** or **Brave Browser**
- **Homebrew**: The macOS package manager.
- **msmtp**: A lightweight SMTP client for sending emails from the command line.

If you don't have Homebrew and msmtp, run these one-shot commands:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install msmtp
```

---

## 2. Configuration

### 2.1. Configure Kindle Email

The first time you run the script, it will ask for your Kindle email. This is a one-time setup.

### 2.2. Configure `msmtp`

This tool uses a secure Google App Password to send emails. You must set this up before running the script.

1. Go to your [Google Account's App Passwords page](https://myaccount.google.com/apppasswords).
2. Select **Mail** for the app and **Other (Custom name)** for the device.
3. Name it `kindle_sender` and click **Generate**.
4. Copy the **16-character password**. You will not see it again.

Next, create the `msmtp` configuration file:

```bash
nano ~/.msmtprc
```

Paste the following content, replacing `<your_gmail_address>` with your Gmail address and `<your_app_password>` with the password you just copied:

```ini
# Gmail account
account default
host smtp.gmail.com
port 587
from <your_gmail_address>
auth on
tls on
tls_certcheck off
user <your_gmail_address>
password <your_app_password>
```

Secure the configuration file:

```bash
chmod 600 ~/.msmtprc
```

---

## 3. Installation

Run this single command to create the `sendkindle` script and make it executable:

```bash
sudo tee /usr/local/bin/sendkindle >/dev/null < /path/to/sendkindle.sh
sudo chmod +x /usr/local/bin/sendkindle
hash -r
echo "The command 'sendkindle' is now ready to use."
echo "Usage: sendkindle <url>"
```

---

## 4. Usage

To run the script, simply call `sendkindle` with a URL as the argument.

```bash
sendkindle https://example.com
```

---

## 5. Expected Output

If the script runs successfully, you will see the following output in your Terminal:

```
Processing URL: https://example.com
Saving PDF to: ~/Desktop/webpage_20250821195929.pdf
PDF created successfully.
Email sent! PDF is on your Desktop.
```

---

## 6. Troubleshooting

If you encounter issues, here are the solutions based on common errors:

| Error                                            | Cause                                              | Solution                                                                             |
| ------------------------------------------------ | -------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `script error: Expected end of line`             | AppleScript syntax issues.                         | The current script uses `msmtp` to bypass this.                                      |
| `Operation timed out`                            | Network firewall blocking SMTP ports (587 or 465). | The `msmtp` setup with App Password bypasses this by using a more robust connection. |
| `msmtp: unrecognized option '--file-attachment'` | Incorrect `msmtp` syntax.                          | The current script uses the corrected `base64` method.                               |
| `base64: invalid argument`                       | Incorrect `base64` syntax.                         | The current script uses the corrected `base64 -i` syntax.                            |
| `unknown: fatal: unable to use my own hostname`  | Postfix is misconfigured.                          | The current script uses `msmtp` to bypass Postfix entirely.                          |

---

## 7. Verification

Since the script reports success on the command line, the best way to verify the email was sent is to check your Gmail account.

- Go to your **Sent** folder in Gmail.
- You should see an email with the subject "convert" and the PDF attached.

This confirms the email has successfully left your computer. 👍
