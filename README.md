secretlounge-ng
---------------
Rewrite of [secretlounge](https://github.com/6697/secretlounge), a bot to make an anonymous group chat on Telegram.
Further rewritten for furry purposes. This version is pseudonymous instead of anonymous (though it still supports anonymous mode).


## @BotFather Setup
1. Message [@BotFather](https://t.me/BotFather)
2. Say "/newbot"
3. Give it a name and an ID
4. Remember to eventually set the userpic and the about text.
5. `/setprivacy`: enabled
6. `/setjoingroups`: disabled
7. `/setcommands`: paste the command list below

### Full Command list
```
start - Join the chat (start receiving messages)
stop - Leave the chat (stop receiving messages)
users - Find out how many users are in the chat
info - Get info about your account
exposeto - Send your real username to someone else
tsign - Sign a message with your tripcode
t - Alias of tsign
motd - Show the welcome message
version - Get version & source code of this bot
modhelp - Show commands available to moderators
adminhelp - Show commands available to admins
toggledebug - Toggle debug mode (sends back all messages to you)
togglekarma - Toggle karma notifications
tripcode - Show or set a tripcode for your messages
tripcodetoggle - Toggle tripcode to be on by default on messages
```
(By default, tripcodetoggle is turned off in config.)
(There are other commands, but they're part of /modhelp and /adminhelp)

### Trimmed Command List for ease of use
```
start - Join the chat (start receiving messages)
stop - Leave the chat (stop receiving messages)
users - Find out how many users are in the chat
info - Get info about your account
exposeto - Send your real username to someone else
motd - Show the welcome message
tripcode - Show or set a tripcode for your messages
```

## Running on a server
1. Get a server somewhere.
2. Brush up on terminals! There are SSH programs you can use on Windows that might be graphical. You can use Windows Subsystem for Linux (WSL) to run ubuntu or such from a cmd window. Otherwise you'll know all this stuff already.
3. SSH into root at your server's address.
4. Make a new user account to run the bots, give them sudoing rights (`usermod -aG sudo __whomever__`)
5. You can use sftp to transfer files. On Windows, just get WinSCP and set up a connection to that new account at the server's IP address. Drag all the files across.
6. SSH into the new user account. You might need to `sudo apt-get update` and then `sudo apt-get install python3-pip`. Use pip3 to install requirements.txt.
7. Copy default configuration from `config.yaml.example` to `bot1/config.yaml`. Edit `bot1/config.yaml` and paste in your bot key from BotFather.
8. Turn the python file into a program: `sudo chmod 755 secretlounge-ng`.
9. You'll want to run it on the server and close the SSH window, so get `sudo apt-get install screen`.

### Running:
1. `screen -dmS bot1` where 'bot1' can be any name you choose.
2. `./secretlounge-ng -c bot1/config.yaml`
3. On your keyboard, press `Ctrl + A`, then press `D` to leave that screen.
4. You can now `exit` and close your session

### Shutting it down:
1. `screen -r bot1` to resume
2. On your keyboard, press `Ctrl + C` to stop the program

## Create another bot
2. Make a `bot2` directory and copy `config.yaml.example` to `bot2/config.yaml`
3. Edit `bot2/config.yaml` and paste in your new bot key from BotFather. Also change 'bot1' to 'bot2' on the database line.
4. `screen -dmS bot2`
5. When you start secretlounge-ng, use the -c flag: `./secretlounge-ng -c bot2/config.yaml`

## Security
You should harden your server by doing a few other things:
- Disable SSH for root
- Close most of the ports. Change defaults for SSH and SFTP

## FAQ

1. **How do I unban a blacklisted user from my bot?**

You can use `/unblacklist 12345678` with those numbers replaced by their username or ID, but be careful you don't accidentally send that info out to everyone.

To do it securely, you can run a script from your server. You need their Telegram User ID (preferred) or username/profile name.
If you have a name you can use `./util/blacklist.py find` to search your bot's database for the user record.

You can then run `./util/blacklist.py unban 12345678` to remove the ban.

(We could add a button interface, but the blacklist could grow far too large for that.)

2. **How do I whitelist a user for my bot?**

Just use `/whitelist` and choose the user from the buttons. You can also do it before they try to join by using `/whitelist 12345678` with those numbers replaced by their user ID. Be careful any time you're using IDs in commands.

3. **How do I demote someone I promoted to mod/admin at some point?**

You can reply to a message with `/blacklist` and then use `/unblacklist @username` (or ID). Otherwise you'll have to do it on the server.

If you have their user ID, you can skip this step: 
Either use the find utility like explained above or run
`./util/perms.py list` to list all users with elevated rank.

Simply run `./util/perms.py set 12345678 user` to remove the users' privileges.

This can also be used to grant an user higher privileges by exchanging the last argument with "*mod*" or "*admin*".

This is all silly. There'll be a /demote command in the future.

4. **What is the suggested setup to run multiple bots?**

The administrative scripts support a structure like the following where each bot has its' own subdirectory:

```
secretlounge folder
\-- bot1
  \-- db.sqlite
  \-- config.yaml
\-- bot2
  \-- db.sqlite
  \-- config.yaml
\-- ...
\-- README.md
\-- secretlounge-ng
```
