
# Connect R and Github

1. Create an account on GitHub.  
2. Install Git locally.  
3. Install R and Rstudio.  


<p>
Use the commands `which git` or `git --version` on the terminal/command line to check if git is already installed. 
Install git using appropriate instructions for Windows or Mac systems.  

The secure shell protocol or SSH helps connect with and authenticate remote servers and services. To setup SSH:  

1) a public/private SSH key pair must be available or generated    
2) the private key must be added to the SSH agent, which is a helper program that tracks user's keys and passphrases   
3) the public SSH key must must be added to the Github account    
</p>  

#### Check for existing SSH keys
<p>
At the Git bash or terminal, your $HOME folder, type `ls -al ~/.ssh` to see if there are files in the .ssh folder.

+ If the key pair exists, you'll see files such as `<filename>` and `<filename.pub>`. 
The key pair could be added to the ssh agent. 

+ If the `.ssh` folder doesn't exist, create one using `mkdir .ssh` and then the private and public key pair can be generated. 

In the steps below, the key pair name is **github**.
</p>

```
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/xxxx/.ssh/id_rsa): github
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

### Connect remote repository on GitHub and Rstudio

1. Create a repository on Github or use an existing one to connect to Rstudio.   
2. Open Rstudio. Go to **Tools > Global Options**, and click on the *Git/SVN section*. Fill in the git executable path and click *OK*.   
3. Use the Code > SSH git link from Github repository to clone or create a project or repository locally.   

For detailed steps checkout [GitHub Docs for connecting with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

