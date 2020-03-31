# Create SSH Keys and add them to GitLab

## Requirements

OpenSSH client should be installed on your system

### Check a version of OpenSSH on macOS 

```
/usr/bin/ssh -V
```
OpenSSH_7.9p1, LibreSSL 2.7.3

## Git

### Check if Git has already been installed

```
git --version
```
** If you don’t receive a “Git version” message, it means that you need to download Git.

### Add your Git username and set your email
```
git config --global user.name "YOUR_USERNAME"
```
To verify:
```
git config --global user.name
```
```
git config --global user.email "your_email_address@example.com"
```
To verify:
```
git config --global user.email
```

To verify that you entered your email correctly, type:
```
git config --global --list
```
You will see the below in your terminal:<br />
user.name=YOUR_USERNAME<br />
user.email=your_email_address@example.com<br />
(END)<br />

To quit:<br />
:q

[source](https://docs.gitlab.com/ee/gitlab-basics/start-using-git.html#open-a-shell)

## ED25519 SSH keys
You should always favor ED25519 SSH keys, since they are more secure and have better performance over the other types.

## Create a new ED25519 SSH key pair

OpenSSH > v7.8
1. Open a terminal on Linux or macOS, or Git Bash / WSL on Windows.<br />
2. Generate a new ED25519 SSH key pair:
  ```
  ssh-keygen -t ed25519 -C "email@example.com"
  ```
3. Next, you will be prompted to input a file path to save your SSH key pair to. <br />
If you don’t already have an SSH key pair and aren’t generating a deploy key, use the suggested path by pressing Enter. <br />
Using the suggested path will normally allow your SSH client to automatically use the SSH key pair with no additional configuration.<br />
If you already have an SSH key pair with the suggested file path, you will need to input a new file path and declare what host this SSH key pair will be used for in your ~/.ssh/config file.<br />

4. Once the path is decided, you will be prompted to input a password to secure your new SSH key pair. <br />
It’s a best practice to use a password, but it’s not required and you can skip creating it by pressing Enter twice.<br />
If, in any case, you want to add or change the password of your SSH key pair, you can use the -p flag:  
```
ssh-keygen -p -f <keyname>
```

## Adding an SSH key to your GitLab account
Copy your public SSH key to the clipboard by using one of the commands below depending on your Operating System:<br />
macOS:
```
pbcopy < ~/.ssh/id_ed25519.pub
```

## Add your public SSH key to your GitLab account by:
1. Clicking your avatar in the upper right corner and selecting Settings.<br />
2. Navigating to SSH Keys and pasting your public key from the clipboard into the Key field. If you:<br />
    * Created the key with a comment, this will appear in the Title field.<br />
    * Created the key without a comment, give your key an identifiable title like Work Laptop or Home Workstation.<br />
3. Choose an (optional) expiry date for the key under “Expires at” section.<br />
4. Click the Add key button.<br />

## Testing that everything is set up correctly
To test whether your SSH key was added correctly, run the following command in your terminal (replacing gitlab.com with your GitLab’s instance domain):
```
ssh -T git@gitlab.com
```

The first time you connect to GitLab via SSH, you will be asked to verify the authenticity of the GitLab host you are connecting to. For example, when connecting to GitLab.com, answer yes to add GitLab.com to the list of trusted hosts:
```
The authenticity of host 'gitlab.com (35.231.145.151)' can't be established.
ECDSA key fingerprint is SHA256:HbW3g8zUjNSksFbqTiUWPWg2Bq1x8xdGUrliXFzSnUw.<br />
Are you sure you want to continue connecting (yes/no)? yes<br />
Warning: Permanently added 'gitlab.com' (ECDSA) to the list of known hosts<br /><br />
```
Note: For GitLab.com, consult the SSH host keys fingerprints, to make sure you’re connecting to the correct server.<br />
Once added to the list of known hosts, you won’t be asked to validate the authenticity of GitLab’s host again. Run the above command once more, and you should only receive a Welcome to GitLab, @username! message.
If the welcome message doesn’t appear, run SSH’s verbose mode by replacing -T with -vvvT to understand where the error is.

[source](https://docs.gitlab.com/ee/ssh/README.html#generating-a-new-ssh-key-pair)
