---
layout: post
title: SSH key authentication
categories: [Tutorial, SSH]
modified: 2016-12-10
comments: true
---

There are many tutorials on creating and configurations SSH Key Authentication. All of them teach the basics, so within 5 minutes you are ready to connect to your remote machine. This is a bit more advanced article that will also allow you support multiple ssh keys and use friendly names for accessing them.

<!--more-->

# 1. Generate a Key Pair

Having you terminal opened on the local machine enter `ssh-keygen`. The command will ask you for a path where the key should be generated.

**Don't use a default name for the key**. Using custom names allows you to create as many keys as you want in an elegant and clear way.


Enter your own path, such as `~/.ssh/yourproject_rsa`, substituting your project's name.

After this you will be asked to add a passphrase. It is optional, but adding one will provide an extra layer of security.

The passphrase is just a key used to encrypt the file that contains the RSA key, using a symmetric cipher (usually DES or 3DES). In order to use the key for public-key encryption, you first need to decrypt its file using the decryption key. ssh does this automatically by asking your for the passphrase.

**If somebody got a hold of the key's file, they wouldn't be able to use it unless they knew the passphrase used to encrypt the file.**

Now you can check you `~/.ssh` directory. There should be two files: `yourproject_rsa` and `yourproject_rsa.pub`. The `.pub` file contains the public key, and the other file contains the private one.

Copy the entire contents of the public key file (`yourproject_rsa.pub` in our example) to the clipboard.

# 2. Copy the Public Key to the Remote Machine

On you remote machine switch to the user you want to use with `su - username`. Now, in you users's home directory create a `.ssh` folder and restrict its permissions :

    mkdir .ssh
    chmod 700 .ssh

Next, open a file in the `.ssh` folder named `authorized_keys`, paste the public key from the local machine into it, and save it.

Don't forget to restrict the permissions of `.ssh/authorized_keys` with `chmod 600 .ssh/authorized_keys`.
 
# 3. Configure SSH on the Local Machine

On your local machine navigate to `.ssh` with cd `~/.ssh`. Here you need to create a `config`:

    nano config

Use the following template to fill it:

    Host [friendly-name]
    Hostname [ip.of.remote]
    Port [####]
    IdentityFile [~/.ssh/private_key_file]

Replace the placeholders above (anything surrounded by brackets, but omit the brackets in the file) with your own information.

Note that you can use multiple host names, so record `Host [friendly-name-0] [friendly-name-1]` is fully acceptable. Lately `friendly_name` will be used to connect to remote machine by ssh:

    ssh friendly-name-0

or

    ssh friendly-name-1

The default port is 22, but for security purposes, it is recommended to change it to some other port between 1025 and 65536.

If your username on the local machine and droplet differ, an additional User field can be added to config to specify the remote user name:

    User yourremoteusername

---

**Note**

It is possible to add as many servers as you like using this template:

    Host myshortname realname.example.com
        HostName realname.example.com
        IdentityFile ~/.ssh/realname_rsa # private key for realname
        User remoteusername
    
    Host myother realname2.example.org
        HostName realname2.example.org
        IdentityFile ~/.ssh/realname2_rsa
        User remoteusername

# 4.Configure SSH on the Remote Machine

In the terminal connected to the remote machine, open the file `/etc/ssh/sshd_config`:

    nano /etc/ssh/sshd_config

Find the line that contains `Port 22` and change the number to match what you put in the Port line of `.ssh/config` in the previous step.

In the same file, find the line that says `PermitRootLogin yes` and change it to `PermitRootLogin no`. This disallows using root as the username to login as via ssh, and is a preventative security measure. Save and exit the file (`Ctrl+x`, `y`, `Enter` from nano).

Restart ssh with:

    service ssh restart

# 5. Test SSH Connection

On our local machine we need to add the key we've been configuring:

    ssh-add ~/.ssh/yourproject_rsa

Don't forget to replace `yourproject_rsa` with your key name.

Now you can connect to your remote machine by running:

    ssh shortname

where `shortname` is one of the friendly names we provided in `Host` record in our `config` file.

If successful, you should be logged into the remote machine without having to type a password (unless you provided an SSH passphrase).

# 6. Disable Password Authentication

To improve the system security even further, you can enforce key-based authentication by disabling the standard password authentication.

Be sure to backup the private SSH key to a safe place if disabling password authentication, as it will be the only way to access the server.

Having logged into the server with ssh, open `/etc/ssh/sshd_config`:

    sudo nano /etc/ssh/sshd_config

Inside the file, search for a directive called `PasswordAuthentication`. This may be commented out. Uncomment the line and set the value to `no`. This will disable your ability to log in through SSH using account passwords:

    PasswordAuthentication no


Save and close the file. Now we need to restart the ssh service:

    sudo service ssh restart

# 7. That's it!
Now everything has been configured and our ssh key authentication is ready to use.