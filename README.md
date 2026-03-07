# VPS via GitHub Actions

This repository provides a GitHub Actions workflow that spins up a temporary VPS (Ubuntu runner) and gives you a live SSH connection so you can log in directly from your terminal.

## How it works

The **VPS SSH Access** workflow:

1. Provisions an Ubuntu runner on GitHub Actions.
2. Installs and starts an OpenSSH server on the runner.
3. Creates a temporary SSH tunnel so the runner can be reached from Termux or any terminal.
4. Prints the SSH username, host, host/IP, and port in the workflow logs.
5. Keeps the session alive for the requested number of minutes (default: 60, max: 360).

## Usage

### 1. Trigger the workflow

1. Go to the **Actions** tab of this repository.
2. Select **VPS SSH Access** from the left-hand list.
3. Click **Run workflow**.
4. (Optional) Set the **Session timeout in minutes** input (1–360). Default is 60.
5. Click **Run workflow** to start.

### 2. Get the SSH connection details

1. Open the running workflow job.
2. Expand the **"Start SSH session and display SSH login info"** step.
3. You will see output similar to:

```
============================================
      VPS SSH CONNECTION DETAILS
============================================

>>> CONNECT FROM TERMUX / TERMINAL <<<
Use these SSH details in Termux:

SSH username: XXXXXXXXXXXXXXXX
SSH host: nyc1.tmate.io
SSH host/IP: 203.0.113.10
SSH port: 22

Termux command:
  ssh XXXXXXXXXXXXXXXX@203.0.113.10 -p 22

--------------------------------------------

>>> RUNNER CREDENTIALS <<<
Username: runner
Password: <generated-password>

(Use these credentials for sudo once
 connected to the SSH session.)

Session timeout: 60 minutes
============================================
```

### 3. Connect via SSH (Termux / Terminal)

Use the SSH username, host/IP, and port shown in the logs:

```bash
ssh XXXXXXXXXXXXXXXX@203.0.113.10 -p 22
```

No separate SSH password is required for that login command.

If the workflow shows a hostname like `nyc1.tmate.io` instead of a numeric IP, use that hostname directly in the same command format.

Once connected, use the **Runner credentials** from the logs for `sudo` commands.

### 4. End the session

The session ends automatically when the timeout expires. To end it early, cancel the running workflow run from the GitHub Actions UI.

## Notes

- Each session uses a **fresh, ephemeral Ubuntu runner**. Nothing persists between sessions.
- The runner has `sudo` access, so you can install any software you need.
- The workflow logs show the SSH username, host, host/IP, and port you need for Termux.
- The runner password is randomly generated per session and displayed in the workflow logs. Use it for `sudo` once connected.
- Session duration is limited by GitHub Actions' maximum job runtime (6 hours).
