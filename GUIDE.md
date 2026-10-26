# Guide: Using Multiple GitHub Accounts on Windows with SSH

## 1. Introduction

This guide explains how to configure multiple GitHub accounts on one Windows computer using separate SSH keys. It allows you to push code to different GitHub accounts without repeatedly switching credentials.

## 2. Prerequisites

- Windows 10 or Windows 11
- Git for Windows installed
- Two GitHub accounts
- Access to the repositories you want to push to

Verify Git and SSH:

    git --version
    ssh -V

## 3. Check the SSH Directory

Run:

    dir %USERPROFILE%\.ssh

Keep any existing SSH keys. Do not overwrite your first account's private key.

## 4. Generate a Second SSH Key

Replace `second-email@example.com` with the email associated with your second GitHub account.

    ssh-keygen -t ed25519 -C "second-email@example.com" -f "%USERPROFILE%\.ssh\id_ed25519_second"

When prompted, enter a passphrase or press Enter twice to leave it empty.

This creates:

- `id_ed25519_second` — Private key
- `id_ed25519_second.pub` — Public key

Never share the private key.

## 5. Add the Public Key to GitHub

Display the public key:

    type %USERPROFILE%\.ssh\id_ed25519_second.pub

Copy the complete output.

1. Sign in to your second GitHub account.
2. Open [GitHub SSH Key Settings](https://github.com/settings/keys).
3. Click **New SSH key**.
4. Enter a title, such as `Windows Second Account`.
5. Paste the public key and save it.

## 6. Configure the SSH Alias

Open the SSH configuration file:

    notepad %USERPROFILE%\.ssh\config

If prompted to create the file, choose **Yes**.

Add the following configuration:

    Host github-second
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_ed25519_second
        IdentitiesOnly yes

Save the file as `config`, without the `.txt` extension.

If the file was saved as `config.txt`, rename it:

    ren "%USERPROFILE%\.ssh\config.txt" config

Verify the configuration:

    type %USERPROFILE%\.ssh\config

## 7. Test the SSH Connection

Run:

    ssh -T git@github-second

Verify the displayed host fingerprint against [GitHub's official SSH fingerprints](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints) before accepting a new host.

If verified, type `yes` when prompted.

A successful connection displays a greeting similar to:

    Hi SECOND_USERNAME! You've successfully authenticated, but GitHub does not provide shell access.

Confirm that the username is your second GitHub account.

## 8. Configure a Repository

Navigate to your project directory. For example:

    cd /d D:\Work\Repositories\timesheets

Check the existing remote:

    git remote -v

Change the remote to use your second account:

    git remote set-url origin git@github-second:USERNAME/REPOSITORY.git

Replace `USERNAME` with the repository owner's username and `REPOSITORY` with the repository name.

For example:

    git remote set-url origin git@github-second:bharathsvri/timesheets.git

If `origin` does not exist, add it:

    git remote add origin git@github-second:USERNAME/REPOSITORY.git

Verify the remote URL:

    git remote -v

The URL should start with `git@github-second:`.

The repository must exist on GitHub, and your account must have permission to push to it.

## 9. Configure the Commit Identity

Set the name and email for the current repository:

    git config --local user.name "Second Account Name"
    git config --local user.email "second-email@example.com"

Verify:

    git config --local user.name
    git config --local user.email

These settings control commit author information. The SSH key and remote URL determine authentication.

## 10. Commit and Push

Check your branch and working-tree status:

    git branch --show-current
    git status

If you have uncommitted changes:

    git add .
    git commit -m "Update project"

If your changes are already committed, do not create another commit unnecessarily.

Push the current branch:

    git push -u origin HEAD

## 11. Configure Another Repository

For another project, update that repository's remote URL.

Example:

    cd /d D:\Work\Repositories\technical-notes
    git remote set-url origin git@github-second:bharathsvri/technical-notes.git
    git push -u origin HEAD

This assumes the repository exists under `bharathsvri` and the account has write access.

Only repositories configured with `git@github-second:` use this alias. Other repositories remain unaffected.

## 12. Troubleshooting

### Could Not Resolve Hostname

Error:

    ssh: Could not resolve hostname github-second

Check the configuration file:

    type %USERPROFILE%\.ssh\config

Ensure the file is named `config`, not `config.txt`, and contains the correct alias.

### Permission Denied (Publickey)

Error:

    Permission denied (publickey).

Check that:

- The public key is added to the correct GitHub account.
- The private key path is correct.
- The SSH configuration is saved properly.

Test again:

    ssh -T git@github-second

### Repository Not Found

Check the remote:

    git remote -v

Confirm that the repository owner and name are correct, that the repository exists, and that the account has write access.

### Wrong GitHub Account

Check that the remote uses the SSH alias:

    git remote set-url origin git@github-second:USERNAME/REPOSITORY.git
    ssh -T git@github-second

Confirm the SSH greeting before pushing.

## 13. Useful Commands

| Purpose | Command |
|---|---|
| List SSH files | `dir %USERPROFILE%\.ssh` |
| Display public key | `type %USERPROFILE%\.ssh\id_ed25519_second.pub` |
| View SSH configuration | `type %USERPROFILE%\.ssh\config` |
| Test second account | `ssh -T git@github-second` |
| View remote URLs | `git remote -v` |
| Change remote URL | `git remote set-url origin git@github-second:USERNAME/REPOSITORY.git` |
| Set commit name | `git config --local user.name "Name"` |
| Set commit email | `git config --local user.email "email@example.com"` |
| Push current branch | `git push -u origin HEAD` |

## 14. Security Best Practices

- Never share or upload your private SSH key.
- Add only public keys (`.pub` files) to GitHub.
- Verify SSH host fingerprints before accepting a new host.
- Confirm repository permissions before pushing.
- Use the correct remote URL for each account.

## 15. Summary

1. Generate a separate SSH key for the second account.
2. Add the public key to GitHub.
3. Configure the SSH alias.
4. Test the SSH connection.
5. Update the repository's remote URL.
6. Set the repository's commit identity.
7. Push using `git push -u origin HEAD`.

Your other repositories remain unaffected unless you change their remote URLs.
