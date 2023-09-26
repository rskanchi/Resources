
# Connect R and Github

### 1. Create a [GitHub](https://github.com/) account: You must know your username, password and the email address used to create the GitHub account.

### 2. Check if you have Git installed.

<p>
Use the commands ```which git``` or ```git --version``` on the terminal/command line to check if git is already installed. Install git using appropriate instructions for Windows or Mac systems.

The secure shell protocol or SSH helps connect with and authenticate remote servers and services. To setup SSH, 1) a public/private SSH key pair must be available or generated; 2) the private key must be added to the SSH agent, which is a helper program that tracks user's keys and passphrases; 3) the public SSH key must must be added to the Github account. This pair of keys are characters.

#### Check for existing SSH keys

If the .ssh folder doesn't exist, it can be created by typing `mkdir .ssh` and the private and public key pair generated. But if the .ssh folder exists, existing SSH keys could be listed to check: 

+ At the Git bash or terminal, your $HOME folder, type `ls -al ~/.ssh` to see if there are files in the .ssh folder.

+ If the .ssh folder contains a public/private key pair, files with same names but one with **.pub**, the key pair could be added to the ssh agent. If the folder doesn't contain the key pair, a new key pair can be generated. In the steps below, I use the key pair name **github**.

```
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/u237865/.ssh/id_rsa): github
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in github.
Your public key has been saved in github.pub.
```

#### Add private SSH key to the SSH-agent

+ Start the SSH-agent `eval "$(ssh-agent -s)"`
+ Edit the **~/.ssh/config** file to automate loading keys into the SSH-agent. 
```
touch ~/.ssh/config # If the config file doesn't exist, create it
vi ~/.ssh/config

Host *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/github # added this line to the already existing config file
  IdentityFile ~/.ssh/cluster
```
+ Add keys to the SSH agent
```
ssh-add ~/.ssh/github
eval "$(ssh-agent -s)"
```

If this setup is on a cluster, you may need to paste the two lines above in the **.bash_profile**.

Ensure git configuration using ```git config -l``` in terminal (Mac) or git bash (Windows). If the username and email are not set, you can use the following to set them:

```
git config --global user.name your_github_username
git config --global user.email email_used@service.com
```

#### Add public SSH key to the Github account
+ Logon to Github
+ Click on the profile photo on top-right, go to **Settings** and then to **SSH and GPG keys**.
+ Click on **New SSH key**, name the key in the *Title* box and copy the contents of the **.pub** public key in the *Key* box.

### 3. Install [R](https://www.r-project.org/) and [Rstudio/posit](https://posit.co/).

### 4. Connect remote repository on GitHub and Rstudio.

If you don't have one yet, create a repository on Github. Open/restart Rstudio. Go to **Tools > Global Options**, and click on the *Git/SVN section*. Fill in the git executable path and click *OK*.


Use the Code > SSH git link from Github repository to clone or create a project or repository locally.

For detailed steps checkout [GitHub Docs for connecting with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).
