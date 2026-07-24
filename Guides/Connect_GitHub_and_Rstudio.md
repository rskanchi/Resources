# Connect R and GitHub

- Create an account on GitHub 
- Install R and RStudio 
- Install Git locally 

Use the commands `which git` or `git --version` on the terminal/command line to check if git is already installed.

---

### Install Git (Mac)

On a Mac, running `git --version` when git isn't installed will prompt you to install the Xcode Command Line Tools (which include git). If no popup appears automatically, trigger it manually:

```
xcode-select --install
```


This opens a separate installer window and takes 5–10 mins to install. Once it finishes, confirm with:


```
git --version
```

### Set Git identity

This sets the name and email that will be attached to your commits. This is separate from your GitHub username/login.

```
git config --global user.name "Your Full Name"
git config --global user.email "your_github_email@example.com"
```

Use the email associated with your GitHub account (check under GitHub > Settings > Emails if unsure). You can verify your settings anytime with `git config -l`.


### Setting up SSH

The secure shell protocol (SSH) helps connect with and authenticate remote servers and services. To set up SSH:

- a public/private SSH key pair must be available or generated
- the private key must be added to the SSH agent, which is a helper program that tracks your keys and passphrases
- the public SSH key must be added to the GitHub account

#### Check for existing SSH keys

At the Git bash or terminal, in your $HOME folder, type `ls -al ~/.ssh` to see if there are files in the .ssh folder.

- If the key pair exists, you'll see files such as `<filename>` and `<filename.pub>`. The key pair could be added to the ssh agent (skip to the [Existing SSH key](#if-you-already-have-an-existing-ssh-key) section below).

- If the `.ssh` folder doesn't exist, that's expected on a new machine. The (`ssh-keygen`) will create it automatically.

#### Generate a new SSH key pair

GitHub's current recommended key type is <b>ed25519</b> (a modern elliptic-curve algorithm which is smaller, faster, and more secure than the older RSA default).

```
ssh-keygen -t ed25519 -C "your_github_email@example.com"
```

- **Enter a file in which to save the key**: press Enter to accept the default location (`~/.ssh/id_ed25519`)
- **Enter passphrase**: press Enter to skip, or set one for extra security
- **Enter same passphrase again**: press Enter (or repeat what you typed)

A "randomart image" will print after key generation. This is just a visual fingerprint of the key and doesn't need to be saved.

#### Add the key to the SSH agent

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

#### Add public SSH key to the GitHub account

- Copy the public key to your clipboard: `pbcopy < ~/.ssh/id_ed25519.pub` (Mac)
- Log on to GitHub
- Click on the profile photo on top-right, go to **Settings**, then **SSH and GPG keys**
- Click **New SSH key**
- Name the key in the *Title* box (e.g. your laptop model) and paste the public key into the *Key* box
- Under **Key type**, select **Authentication Key** (default) — this is what's needed for clone/push/pull. **Signing Key** is a separate option used only to show commits as "Verified" on GitHub.
- Click **Add SSH key**

#### Test the SSH connection

```
ssh -T git@github.com
```

The first time you connect, you'll see a host authenticity warning. Type <code>yes</code> to continue. You should then see:

```
Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
```

### Connect remote repository on GitHub and RStudio

- Create a repository on GitHub or use an existing one
- Optional: (usually auto-detected once git is installed, otherwise) Open RStudio. Go to **Tools > Global Options**, click the *Git/SVN* section, and confirm the git executable path is filled in 
- Clone the repo using its SSH URL (Code > SSH tab on the GitHub repo page):

```
git clone git@github.com:username/repo-name.git
```

- To get the **Git tab** to appear in RStudio's top-right panel, the cloned folder needs to be opened as an RStudio Project:
   - If the folder contains an `.Rproj` file: **File > Open Project...** > select it
   - If not: **File > New Project > Existing Directory** > browse to the cloned folder > **Create Project**

For detailed steps checkout [GitHub Docs for connecting with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

---

### If you already have an existing SSH key

If `ls -al ~/.ssh` shows an existing key pair, you likely don't need to generate a new one — just make sure it's loaded and registered. This is especially relevant on a shared or previously-used laptop, where an older <b>RSA</b> key pair (`id_rsa` / `id_rsa.pub`) may already exist from a prior setup, rather than the newer `id_ed25519` type generated in the steps above. Check the `.ssh` folder to see which filename(s) are present before proceeding — the steps below work the same either way, just substitute the correct filename.


1. **Add the existing key to the ssh-agent:**
```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```
(use whichever private key filename is present — `id_rsa`, `id_ed25519`, or a custom name like `github`)

2. **(Optional) Automate key loading across sessions** by creating/editing `~/.ssh/config`, especially useful if multiple key types/files exist:
```
touch ~/.ssh/config   # if it doesn't already exist
```
Add the following, listing each key you want auto-loaded:
```
Host *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_rsa
  IdentityFile ~/.ssh/id_ed25519
  IdentityFile ~/.ssh/github
```

3. **Confirm the key is already on GitHub**: GitHub > Settings > SSH and GPG keys. If it's listed, you're done — just run `ssh -T git@github.com` to confirm the connection works. If it's not listed, copy the public key (`pbcopy < ~/.ssh/id_rsa.pub`, substituting the correct filename) and add it following the steps above.

4. If working on a shared machine or cluster, the `eval` and `ssh-add` lines may need to be added to your `.bash_profile` so the key loads automatically on login.
