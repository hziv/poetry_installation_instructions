# Poetry Installation Instructions

Poetry installation instructions on a bare Ubuntu machine.

# Setting up

On a Windows machine:

1. download the latest [Ubuntu Desktop](https://ubuntu.com/download/desktop) `iso` file.
1. download [Oracle VirtualBox](https://www.virtualbox.org) installation `exe`cutable.
1. inestall the Oracle VirtualBox and point the installation to the Ubuntu `iso` file.

# Preparing Ubuntu machine

## Pre-requisits

1. Follow [instructions](https://pipx.pypa.io/stable/installation) (https://github.com/pypa/pipx) to install `pipx`. Specifically for Ubuntu/Linux, run `sudo apt update && sudo apt upgrade -y`, followed by `sudo apt install pipx` and `pipx ensurepath`.
1. Run `sudo pipx ensurepath --global` to allow `pipx` actions in global scope.
1. On Ubuntu/Linux run `eval "$(register-python-argcomplete pipx)"` to add `pipx` to the shell.
1. Run `pipx install poetry` to install `pipx`.
1. Run `pipx upgrade poetry` to update to the latest version.
1. Run `poetry completions bash >> ~/.bash_completion` to add `poetry` to the shell.

## Installing "esentials"

1. Optionally, install NeoVim by running `sudo apt install neovim`. To run NeoVim type `nvim` on the CLI.
1. Install `git` by running `sudo apt-get install git-all`.
1. Run `git config --global user.name "<your_username>"` and `git config global user.email "your@email.address"` to set-up git configuration details.
1. Follow the instructions [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token) to create a GitHub *fine-grained personal access token*. This will be used to gain access to your GitHub repositories.

# General instructions to create project

1. It's highly recommended to `mkdir dev` so that the projects are not directly on your `home` directory. This is just good practice, and not a must.
1. Run `cd dev` followed by `git clone <project_url>`. Get your `<project_url>` from from your [GitHub](https://github.com) account. **IMPORTANT** - when cloaning use your *fine-grained personall access token* instead of a password
1. Enjoy ;-)
