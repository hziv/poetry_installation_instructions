# Poetry Installation Instructions

Poetry installation instructions on a bare Ubuntu machine.

# Setting up

On a Windows (host) machine:

1. download the latest [Ubuntu Desktop](https://ubuntu.com/download/desktop) `iso` file.
1. download [Oracle VirtualBox](https://www.virtualbox.org) installation `exe`cutable.
1. install the Oracle VirtualBox and point the installation to the Ubuntu `iso` file.

# Preparing Ubuntu machine (guest)

## Pre-requisits

1. Follow [instructions](https://pipx.pypa.io/stable/installation) (https://github.com/pypa/pipx) to install `pipx`. Specifically for Ubuntu/Linux, run `sudo apt update && sudo apt upgrade -y`, followed by `sudo apt install pipx` and `pipx ensurepath`.
1. ~~Run `sudo pipx ensurepath --global` to allow `pipx` actions in global scope.~~
1. On Ubuntu/Linux run `eval "$(register-python-argcomplete pipx)"` to add `pipx` to the shell.
1. Run `pipx install poetry` to install `pipx`.
1. Run `pipx upgrade poetry` to update to the latest version.
1. Run `poetry completions bash >> ~/.bash_completion` to add `poetry` to the shell.

## Installing "essentials"

### Setting up ability to SSH into machine

Rational - this is required in order to be able to set up the GitHub account access inside the guest machine (where you will need to apply a token that will be available in the host machine into the guest machine).

1. To **Install SSH in VirtualBox OS** - on the guest Linux machine, open a terminal and run `sudo apt install openssh-server`.
1. Check SSH status by running `sudo systemctl status ssh`.
1. Look for the line: `Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)` in the results.
1. If the above is not `enabled`, start it manually by running `sudo systemctl enable ssh --now`. Now confirm by running step's 2 & 3 above again.
1. To **Open SSH port in Firewall** - on the guest Linux machine run `sudo lsof -i -P -n | grep LISTEN` to get a list of open ports.
1. If in the above result you can't find **port 22**, open the port manually by running `sudo ufw allow ssh` and `sudo ufw status verbose`.
1. **VirtualBox network settings** - In the VirtualBox app on the host machine, allow the SSH connection by navigating to the *VirtualBox settings --> Network* and confirm the *Attached to:* setting is set to *NAT*. Then click the *Port Forwarding* button and set the following settings - Name: `ssh`, Protocol: `TCP`, Host Port: `2222`, Guest port: `22`. The *IP* fields can be left empty.
1. To **Install SSH client** - in the guest Linux machine install the OpenSSH client by running `sudo apt install openssh-client`.
1. You can now connect from the host machine into the guest machine either by PuTTY (with the following settings - *Host Name*: `localhost`, *Port*: `2222`, *Connection Type*: `SSH`).

### Further essentials

1. Optionally, install NeoVim by running `sudo apt install neovim`. To run NeoVim type `nvim` on the CLI.
1. Install `git` by running `sudo apt-get install git-all`.
1. Run `git config --global user.name "<your_username>"` and `git config global user.email "your@email.address"` to set-up git configuration details.
1. Follow the instructions [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token) to create a GitHub *fine-grained personal access token*. This will be used to gain access to your GitHub repositories.
1. Store the *token* in a secure place, and use it in place of a password when `cloan`ing into the guest machine.

# General instructions to create project

1. It's highly recommended to `mkdir dev` so that the projects are not directly on your `home` directory. This is just good practice, and not a must.
1. Run `cd dev` followed by `git clone <project_url>`. Get your `<project_url>` from from your [GitHub](https://github.com) account. **IMPORTANT** - when cloaning use your *fine-grained personall access token* instead of a password
1. Enjoy ;-)
