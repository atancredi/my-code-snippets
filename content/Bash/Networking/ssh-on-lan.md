# `ssh` within a home Local Area Network (LAN)

I assume there are two users each with their own computers running Ubuntu.

-   `user1` uses `comp1` and
-   `user2` uses `comp2`.

The goal is for `user1` to remotely access `comp2` using `user2`'s account in `comp2`.

**Note:** If `user1` has an account in `comp2` and `user1` wishes to access her account in `comp2` from `comp1` then she she does not have to specify the username in the ssh command. `ssh comp2.local` will work.

## Setup `ssh` server in `comp2`

For this you will need to physically go to `comp2` and login to it using an account with administration privileges. I assume that is, `user2` account.

## Install the `ssh` server

Desktop Ubuntu does not come with any servers. To `ssh` into `comp2` you will first need to install the `openssh-server`. Open a terminal in `comp2` by pressing Ctrl+Alt+T and enter the following lines one at a time:

```
sudo apt update
sudo apt install openssh-server
```

You will be asked for `user2`'s password. When you type the password the cursor will not move and it will seem like nothing is happening. This is normal. Hit Enter after typing the password. Then follow the instructions.

Once `openssh-server` is installed you will see a new options in Settings under sharing called "RemoteLogin" and it will be "On":

[![enter image description here](https://i.stack.imgur.com/WXvjQ.png)](https://i.stack.imgur.com/WXvjQ.png)

The standard Ubuntu desktop does not come with any firewalls installed. If you have a firewall installed then make sure it allows connections to port 22 from within the LAN. The instructions will depend on the specific firewall software.

## Test `ssh` locally

Still at the terminal of `comp2` test that `ssh` is working. Enter the command:

```
ssh 127.0.0.1
```

The `127.0.0.1` refers to the IP address of the computer you are using. In other words, you are trying to ssh from `comp2` to `comp2`. If all goes well you will be asked if you are sure you want to connect and then for your password. Once you answer `yes` for being sure and enter the password for `user2` you will see the terminal prompt change from `user2@comp2$` to `user2@127.0.0.1$`. This shows that you have successfully sshed from `comp2` to itself.

**Note:** Since the same user (`user2`) is ssh-ing in this case, you don't need to specify `ssh user2@127.0.0.1` in the ssh command.

## `ssh` from `comp1`

To `ssh` from `comp1` to `comp2` you can either use the computer name (hostname) or its IP address. To find the IP address of `comp2` use the `ifconfig` command in the terminal of `comp2`. You will see an address like `192.168.x.y`, where `x` can be `0` or `1` and `y` can be any number between `2` and `255`.

From a terminal in `comp1` enter either:

```
ssh user2@comp2.local
```

or

```
ssh user2@192.168.x.y
```

**Note:** If you use the name of the computer then you must add `.local` at the end. If you use the local IP address, it may change from time to time if a fixed address is not assigned.

# Security concerns

1.  Router setup
2.  Enable public key based authentication and disable password based authentication.
3.  Install and configure a firewall

## 1. Router setup

Make sure port 22 is not forwarded to any computers in the home router. This will prevent anyone from outside the home LAN use ssh to connect to the home computers.

The instructions are router specific and beyond the scope of this answer as it has nothing to do with Ubuntu.

If you do want set up port forwarding, see [How to access home ssh server from outside via the Internet?](https://askubuntu.com/questions/1360840/how-to-access-home-ssh-server-from-outside-via-the-internet/1360963#1360963)

## 2. Enable public key based authentication and disable password based authentication

This is a more secure way to use ssh. It uses a private-public key pair. The private key remains in the trusted computer from which the ssh connection is made. In this case `comp1`. The public key goes to `comp2`. Once the keys are in place, you will disable password based authentication in the ssh server in `comp2`. If you disable password based authentication without making sure the key based authentication is working, then ssh will not work, as there will be no way to authenticate the remote user.

First generate the private-public key pair in the `user1@comp1`. This will need to be done at each user and each local computer from where you ssh to another computer. In a terminal enter:

```
ssh-keygen -t ed25519
```

This generates the newer and more secure key than RSA. If you want RSA type keys, then enter:

```
ssh-keygen -t rsa -b 4096
```

The process will prompt you for a passphrase. You can hit Enter if you don't want one. If you do enter a passphrase, you will be asked for it every time you ssh from `comp1` to `comp2`. If you use a passphrase it should not be same as the password used for normal login.

Next you will need to copy the public key from `comp1` to `comp2`. In the terminal in `comp1` enter:

```
ssh-copy-id user2@comp2.local
```

You will be asked to enter the login password of `user2` in `comp2`. If you have other computers in the home LAN you want to ssh to from the `user1@comp1` then you need not create a new key-pair. Copy the public key of `user1@comp1` to the other user accounts in the other remote computers using the above command.

Once the public key is successfully copied to the `user2` account of `comp2` try to ssh again:

```
ssh user2@comp2.local
```

Now you should be able to get into `comp2` without `user2`'s password. At this point one can either use the password or the public key you generated to log in. You can test this by creating a new user (or with an existing second user) in `comp1`, such as `user1a`. At this stage `user1@comp1` will be able to ssh to `user2@comp2` without password using the public key. On the other hand `user1a@comp1` will need to use the password of `user2` to ssh to `user2@comp2`.

The next step is to disable the password based authentication. You may want to do this locally in a terminal of `comp2`. Use the following command to edit `/etc/ssh/sshd_config`

sudo nano /etc/ssh/sshd_config

Then make sure it contains the following lines and they are uncommented:

```
PasswordAuthentication no
ChallengeResponseAuthentication no 
```

Note these lines may not be together. "Uncommented" means there is no `#` in front of each of these lines.

Use Ctrl+O to save the changes and Ctrl+X to exit the editor.

Finally, restart the ssh server with the new settings by the following command:

```
sudo systemctl restart ssh
```

Now `user1@comp1` will **still** be able to ssh to `user2@comp2` without password using the public key. On the other hand `user1a@comp1` will get **permission denied** to ssh to `user2@comp2`.

## 3. Install and configure a firewall

There are many firewall software, and some of them are hard to configure. I suggest you install the "uncomplicated firewall" called `ufw` by the command:

```
sudo apt install ufw
```

To open the port 22 but only from within the home LAN use the command:

```
sudo ufw allow from 192.168.x.0/24 to any port 22 
```

**Note:** replace `x` with either `0` or `1` based on your router setup.

This firewall setting in `comp2` stops anyone from outside the home LAN use ssh to connect to `comp2`. However, it allows anyone (using any computer) within the home LAN try to ssh into `comp2`. If `comp2` is removed from home and taken somewhere else and connected to another "similar" network, say by WiFi, all the computers in that network will be allowed to access port 22 of `comp2` by this firewall setting. For this reason I recommend password based authentication to be disabled and private-public key based authentication be used in all computers running the ssh server.

Hope this helps

(https://askubuntu.com/questions/1107987/connect-two-computers-with-ssh-in-a-home-lan)