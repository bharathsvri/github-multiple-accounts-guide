# GitHub Multiple Accounts Guide

A step-by-step guide to configuring multiple GitHub accounts on a single Windows computer using SSH authentication.

## Overview

This project explains how to use multiple GitHub accounts on the same Windows computer without repeatedly switching credentials.

By configuring separate SSH keys and aliases, you can connect each repository to the appropriate GitHub account.

## Features

- Configure multiple GitHub accounts on Windows.
- Generate separate SSH keys for different accounts.
- Configure SSH aliases for each account.
- Connect individual repositories to the correct account.
- Push and pull code using SSH authentication.
- Troubleshoot common Git and SSH errors.

## Prerequisites

- Windows 10 or Windows 11
- Git for Windows installed
- Two or more GitHub accounts
- Access to the repositories you want to use

## Project Structure

    github-multiple-accounts-guide/
    ├── README.md
    ├── GUIDE.md
    ├── LICENSE
    └── .gitignore

## Getting Started

### 1. Generate an SSH Key

Generate a separate SSH key for your second GitHub account.

    ssh-keygen -t ed25519 -C "second-email@example.com" -f "%USERPROFILE%\.ssh\id_ed25519_second"

Replace `second-email@example.com` with the email associated with your second GitHub account.

### 2. Add the Public Key to GitHub

Display your public key:

    type %USERPROFILE%\.ssh\id_ed25519_second.pub

Copy the output and add it to your second GitHub account under:

[GitHub SSH Key Settings](https://github.com/settings/keys)

### 3. Configure the SSH Alias

Open your SSH configuration file:

    notepad %USERPROFILE%\.ssh\config

Add the following configuration:

    Host github-second
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519_second
        IdentitiesOnly yes

Save the file as `config`, without the `.txt` extension.

### 4. Test the Connection

Run:

    ssh -T git@github-second

Confirm that the greeting displays the expected GitHub username.

### 5. Configure Your Repository

Set the repository's remote URL to use the second account:

    git remote set-url origin git@github-second:USERNAME/REPOSITORY.git

Replace `USERNAME` and `REPOSITORY` with the correct repository owner and name.

### 6. Push Your Code

Push the current branch:

    git push -u origin HEAD

For the complete instructions, troubleshooting steps, and security recommendations, see [GUIDE.md](GUIDE.md).

## Useful Commands

| Purpose | Command |
|---|---|
| Check SSH files | `dir %USERPROFILE%\.ssh` |
| View SSH configuration | `type %USERPROFILE%\.ssh\config` |
| Test second account | `ssh -T git@github-second` |
| Check repository remotes | `git remote -v` |
| Push current branch | `git push -u origin HEAD` |

## Troubleshooting

### Repository Not Found

- Verify that the repository exists on GitHub.
- Check the repository owner and name.
- Confirm that your account has permission to push.
- Verify that the remote URL uses the correct SSH alias.

### Permission Denied (Publickey)

- Confirm that your public key is registered with the correct GitHub account.
- Check the private key path in your SSH configuration.
- Test the SSH connection again.

### Could Not Resolve Hostname

- Verify that the SSH configuration file is named `config`, not `config.txt`.
- Confirm that the `github-second` alias is configured correctly.

## Security

- Never share your private SSH key.
- Add only public keys (`.pub` files) to GitHub.
- Verify SSH host fingerprints before accepting a new host.
- Ensure each repository uses the intended GitHub account.

## Documentation

Read the complete setup guide:

**[GUIDE.md — Using Multiple GitHub Accounts on Windows with SSH](GUIDE.md)**

## License

See the [LICENSE](LICENSE) file for licensing information.
