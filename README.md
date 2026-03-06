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

### 2. Get the SSH connection string

1. Open the running workflow job.
2. Expand the **"Start tmate session and display SSH connection info"** step.
3. You will see output similar to:

```
============================================
      VPS SSH CONNECTION DETAILS
============================================

SSH command (web):
https://tmate.io/t/XXXXXXXXXXXXXXXX

SSH command (terminal):
ssh XXXXXXXXXXXXXXXX@nyc1.tmate.io

Runner username: runner
Runner password: <generated-password>

Session timeout: 60 minutes
============================================
```

### 3. Connect via SSH

Copy the **SSH command (terminal)** and run it in your local terminal:

```bash
ssh XXXXXXXXXXXXXXXX@nyc1.tmate.io
```

Enter the **Runner password** shown in the logs when prompted.

Alternatively, open the **SSH command (web)** URL in your browser for a browser-based terminal.

### 4. End the session

The session ends automatically when the timeout expires. To end it early, cancel the running workflow job from the GitHub Actions UI.

## Notes

- Each session uses a **fresh, ephemeral Ubuntu runner**. Nothing persists between sessions.
- The runner has `sudo` access, so you can install any software you need.
- The SSH password is randomly generated per session and displayed only in the workflow logs (visible to repository collaborators with Actions access).
- Session duration is limited by GitHub Actions' maximum job runtime (6 hours).
