# VPS via GitHub Actions

This repository provides a GitHub Actions workflow that spins up a temporary VPS (Ubuntu runner) and gives you a live SSH connection so you can log in directly from your terminal.

## How it works

The **VPS SSH Access** workflow:

1. Provisions an Ubuntu runner on GitHub Actions.
2. Installs and starts an OpenSSH server on the runner.
3. Creates a secure tunnel using [tmate](https://tmate.io/) that exposes the runner's SSH port to the internet.
4. Prints the SSH connection string in the workflow logs.
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
2. Expand the **"Start tmate session and display SSH connection info"** step.
3. You will see output similar to:

```
============================================
      VPS SSH CONNECTION DETAILS
============================================

>>> CONNECT FROM TERMUX / TERMINAL <<<
Copy and paste this command into Termux or any terminal:

  ssh XXXXXXXXXXXXXXXX@nyc1.tmate.io

(No password needed — just run the command above)

--------------------------------------------

>>> CONNECT FROM BROWSER <<<
Open this URL in your browser:

  https://tmate.io/t/XXXXXXXXXXXXXXXX

--------------------------------------------

>>> RUNNER CREDENTIALS <<<
Username: runner
Password: <generated-password>

(Use these credentials if prompted for a
 password inside the tmate session, or for
 sudo access once connected.)

Session timeout: 60 minutes
============================================
```

### 3. Connect via SSH (Termux / Terminal)

Copy the SSH command shown in the logs and paste it directly into Termux or any terminal:

```bash
ssh XXXXXXXXXXXXXXXX@nyc1.tmate.io
```

**No password is required** to connect — you will get a shell immediately.

Once connected, use the **Runner credentials** from the logs for `sudo` commands.

### 4. Connect via Browser

Open the **web URL** from the logs in your browser for a browser-based terminal.

### 5. End the session

The session ends automatically when the timeout expires. To end it early, cancel the running workflow run from the GitHub Actions UI.

## Notes

- Each session uses a **fresh, ephemeral Ubuntu runner**. Nothing persists between sessions.
- The runner has `sudo` access, so you can install any software you need.
- The tmate SSH command (`ssh TOKEN@nyc1.tmate.io`) connects you directly — **no password needed**.
- The runner password is randomly generated per session and displayed in the workflow logs. Use it for `sudo` once connected.
- Session duration is limited by GitHub Actions' maximum job runtime (6 hours).
