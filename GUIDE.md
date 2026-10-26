Guide: Using Multiple GitHub Accounts on Windows with SSH

This guide explains how to configure two GitHub accounts on the same Windows computer and push code from different repositories using the appropriate account.

1. Prerequisites
Windows with Git installed.
Two GitHub accounts.
Access to the repositories you want to push to.
Windows Command Prompt (CMD) or a VS Code terminal using CMD.

Check that Git and SSH are installed:

cmd
git --version
ssh -V

2. Check the SSH Folder

Open CMD and run:

cmd
dir %USERPROFILE%\.ssh


If you don't already have an SSH key for your second account, continue to the next step. Keep any existing keys for your first account.

3. Generate an SSH Key for the Second Account

Replace the email with the email associated with your second GitHub account.

cmd
ssh-keygen -t ed25519 -C "second-email@example.com" -f "%USERPROFILE%\.ssh\id_ed25519_second"


When prompted for a passphrase, enter one or press Enter twice to leave it empty.

This creates two files:

id_ed25519_second — private key. Never share this file.
id_ed25519_second.pub — public key. This is the key you add to GitHub.
4. Add the Public Key to GitHub

Display the public key:

cmd
type %USERPROFILE%\.ssh\id_ed25519_second.pub


Copy the entire output.

Sign in to your second GitHub account.
Open GitHub SSH key settings.
Click New SSH key.
Enter a title, such as Windows Second Account.
Paste the public key and save it.
5. Configure the SSH Alias

Open the SSH configuration file:

cmd
notepad %USERPROFILE%\.ssh\config


If prompted to create the file, choose Yes.

Add the following configuration:

sshconfig
Host github-second
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_second
    IdentitiesOnly yes


Save the file without a .txt extension.

Important: If Notepad saves the file as config.txt, rename it in CMD:

cmd
ren "%USERPROFILE%\.ssh\config.txt" config


Verify the filename:

cmd
dir %USERPROFILE%\.ssh


The file must be named config, not config.txt.

6. Test the Second Account

Run:

cmd
ssh -T git@github-second


On the first connection, SSH may ask you to confirm the server's authenticity. Verify the displayed fingerprint against GitHub's official SSH fingerprints before accepting it.

A successful connection displays a greeting similar to:

text
Hi SECOND_USERNAME! You've successfully authenticated, but GitHub does not provide shell access.


Confirm that SECOND_USERNAME is the expected GitHub account.

7. Configure a Repository to Use the Second Account

Navigate to the project directory. For example:

cmd
cd /d D:\Work\Repositories\timesheets


Check the current remote:

cmd
git remote -v


If the remote uses HTTPS or the default GitHub hostname, change it to the second account's SSH alias:

cmd
git remote set-url origin git@github-second:SECOND_USERNAME/REPOSITORY_NAME.git


Replace:

SECOND_USERNAME with the repository owner's GitHub username.
REPOSITORY_NAME with the repository name.

For example, if the repository belongs to bharathsvri and is named timesheets:

cmd
git remote set-url origin git@github-second:bharathsvri/timesheets.git


If no origin remote exists, add it instead:

cmd
git remote add origin git@github-second:SECOND_USERNAME/REPOSITORY_NAME.git


Verify the remote:

cmd
git remote -v


The URL should start with git@github-second:.

Note: The repository must exist on GitHub, and the selected account must have permission to push to it.

8. Configure the Commit Identity for This Repository

Run these commands inside the project directory:

cmd
git config --local user.name "Second Account Name"
git config --local user.email "second-email@example.com"


Verify the configuration:

cmd
git config --local user.name
git config --local user.email


These settings apply only to the current repository. They determine the commit author information, not the account used for authentication.

9. Push Your Code

Check the current branch and status:

cmd
git branch --show-current
git status


If you have uncommitted changes, stage and commit them:

cmd
git add .
git commit -m "Update project"


If you have already committed your changes, do not create another commit unnecessarily.

Push the current branch:

cmd
git push -u origin HEAD


The command pushes the current branch to the configured remote.

10. Configure Another Repository

Repeat Section 7 for each repository that should use the second GitHub account.

For example, for a repository named technical-notes:

cmd
cd /d D:\Work\Repositories\technical-notes
git remote set-url origin git@github-second:bharathsvri/technical-notes.git
git push -u origin HEAD


This assumes the repository exists under bharathsvri and that account has write access.

Only repositories configured with git@github-second: use this SSH alias. Other repositories remain unaffected.

11. Troubleshooting
Error: Could not resolve hostname github-second

Cause: SSH cannot find the alias in the configuration file.

Solution:

cmd
type %USERPROFILE%\.ssh\config


Check that the file exists without a .txt extension and contains the Host github-second configuration.

Error: Permission denied (publickey)

Cause: SSH cannot authenticate with the configured key.

Solution:

Confirm the public key was added to the intended GitHub account.
Check the private key path in the SSH configuration.
Test the connection again:
cmd
ssh -T git@github-second

Error: Repository not found

Cause: The repository URL may be incorrect, the repository may not exist, or the account may lack access.

Solution:

cmd
git remote -v


Verify the repository owner and name. Confirm that the repository exists and that the account has write permission.

Error: The wrong GitHub account is used

Cause: The remote may still use HTTPS or the default github.com hostname.

Solution:

cmd
git remote set-url origin git@github-second:SECOND_USERNAME/REPOSITORY_NAME.git
ssh -T git@github-second


Verify the SSH greeting and remote URL before pushing.

12. Useful Commands
Purpose	Command
Check SSH files	dir %USERPROFILE%\.ssh
Display public key	type %USERPROFILE%\.ssh\id_ed25519_second.pub
View SSH configuration	type %USERPROFILE%\.ssh\config
Test second account	ssh -T git@github-second
View remote URLs	git remote -v
Change remote URL	git remote set-url origin git@github-second:USERNAME/REPOSITORY.git
Set commit author	git config --local user.name "Name"
Set commit email	git config --local user.email "email@example.com"
Push current branch	git push -u origin HEAD
Summary
Generate a separate SSH key for the second account.
Add the public key to that GitHub account.
Configure the github-second SSH alias.
Test the connection using ssh -T git@github-second.
Update each intended repository's remote URL.
Configure the local commit identity.
Push with git push -u origin HEAD.

Security reminder: Never share or upload your private SSH key. Only the .pub file should be added to GitHub.
