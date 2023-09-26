
# SSH and Github

The secure shell protocol or SSH helps connect with and authenticate remote servers and services. To setup SSH, 1) a public/private SSH key pair must be available or generated; 2) the private key must be added to the SSH agent, which is a helper program that tracks user's keys and passphrases; 2) the public SSH key must must be added to the Github account. This pair of keys are characters

## Check for existing SSH keys

If the .ssh folder doesn't exist, it can be created by typing `mkdir .ssh` and the private and public key pair generated. But if the .ssh folder exists, existing SSH keys could be listed to check: 

1. At the terminal, your $HOME folder, type `ls -al ~/.ssh` to see if there are files in the .ssh folder.

2. If the .ssh folder contains a public/private key pair, files with same names but one with **.pub**, the key pair could be added to the ssh agent. If the folder doesn't contain the key pair, a new key pair can be generated. In the steps below, I use the key pair name **github**.

```
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/u237865/.ssh/id_rsa): github
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in github.
Your public key has been saved in github.pub.
```

## Add private SSH key to the SSH-agent

1. Start the SSH-agent `eval "$(ssh-agent -s)"`
2. Edit the **~/.ssh/config** file to automate loading keys into the SSH-agent. 
```
touch ~/.ssh/config # If the config file doesn't exist, create it
vi ~/.ssh/config

Host *
  AddKeysToAgent yes
  IdentityFile ~/.ssh/github # added this line to the already existing config file
  IdentityFile ~/.ssh/cluster
```
3. Add keys to the SSH agent
```
ssh-add ~/.ssh/github
eval "$(ssh-agent -s)"
```

If this setup is on a cluster, you may need to paste the two lines above in the **.bash_profile**.

## Add public SSH key to the Github account
1. Logon to Github
2. Click on the profile photo on top-right, go to **Settings** and then to **SSH and GPG keys**.
3. Click on **New SSH key**, name the key in the *Title* box and copy the contents of the **.pub** public key in the *Key* box.

For detailed steps checkout [GitHub Docs for connecting with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

To clone a repo on your mac, copy the Code > SSH git link.
