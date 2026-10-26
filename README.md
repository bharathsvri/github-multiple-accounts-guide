GitHub Multiple Accounts Guide

A practical guide to using multiple GitHub accounts on a single Windows computer with SSH authentication.

Features
Configure separate SSH keys for multiple GitHub accounts.
Set up SSH aliases for each account.
Connect individual repositories to the correct account.
Push and pull code without repeatedly switching credentials.
Troubleshoot common SSH authentication and repository errors.
Documentation

Read the complete setup instructions in GUIDE.md.

Requirements
Windows 10 or Windows 11
Git for Windows
Two GitHub accounts
SSH authentication
Example
sshconfig
Host github-second
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_second
    IdentitiesOnly yes


Test the connection:

cmd
ssh -T git@github-second

Security

Never share your private SSH keys. Only upload the corresponding public keys to GitHub.

License

See LICENSE for licensing information.
