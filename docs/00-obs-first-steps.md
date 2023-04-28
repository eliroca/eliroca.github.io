title: OBS - first steps with osc

# First steps with osc


### 1. Install osc

openSUSE Commander (osc) is a command-line interface to the Open Build Service (OBS).
On an openSUSE / SUSE distribution, just install it via zypper, openSUSE's package manager.

``` bash
zypper in osc
```

* * *
### 2. Connect to OBS

By default, osc will ask if you have a community account on the openSUSE OBS instance.
Go ahead and sign up if you haven't already ;-) it's at https://build.opensuse.org.

Let's run osc for the first time!

``` hl_lines="1"
osc user

Your user account / password are not configured yet.
You will be asked for them below, and they will be stored in
/root/.config/osc/oscrc for future use.

Creating osc configuration file /root/.config/osc/oscrc ...
Username [api.opensuse.org]: 
Password [eroca@api.opensuse.org]: 

NUM NAME              DESCRIPTION
1   Kernel keyring    Store password in user session keyring in kernel keyring [secure, in-memory, per-session]
2   Secret Service    Store password in Secret Service (GNOME Keyring backend) [secure, persistent]
3   Transient         Do not store the password and always ask for it [secure, in-memory]
4   Obfuscated config Store the password in obfuscated form in the osc config file [insecure, persistent]
5   Config            Store the password in plain text in the osc config file [insecure, persistent]
Select credentials manager [default=1]: 2

eroca: "Elisei Roca" <eroca@suse.com>
```

* * *
### 3. Connect to other OBS instances

At SUSE, we have multiple OBS instances, here's how the command looks like:

``` hl_lines="1"
osc --apiurl https://api.suse.de user

eroca: "Elisei Roca" <eroca@suse.com>
```

<br/>

## Links

* [osc source code](https://github.com/openSUSE/osc)

* [osc documentation](https://en.opensuse.org/openSUSE:OSC)

* [openSUSE OBS instance](https://build.opensuse.org)
