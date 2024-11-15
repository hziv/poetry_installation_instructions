# Poetry Installation Instructions

Poetry installation instructions on a bare Ubuntu (guest) machine, including all required steps.

# Setting up guest system

On the Windows (host) machine:

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

# General instructions to prepare project

1. It's highly recommended to `mkdir dev` so that the projects are not directly on your `home` directory. This is just good practice, and not a must.
1. Run `cd dev` followed by `git clone <project_url>`. Get your `<project_url>` from from your [GitHub](https://github.com) account. **IMPORTANT** - when cloaning use your *fine-grained personall access token* instead of a password
1. Below are step-by-step instructions on how to install and use Poetry, a dependency management and packaging tool for Python, to create a project environment and work on it effectively.

---

### **Step 1: Install Poetry**

Poetry is a Python tool for managing dependencies, packaging, and publishing Python projects. Here's how to install it.

#### 1.1. **Install Poetry using the official installer**

The recommended method to install Poetry is through their official installation script. Open a terminal (or command prompt) and run the following command:

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

This will download and run the installer script. The script will install the latest version of Poetry for your system.

#### 1.2. **Verify the Installation**

Once the installation is complete, you can verify that Poetry has been installed correctly by checking its version:

```bash
poetry --version
```

If Poetry is installed correctly, you should see something like:

```
Poetry version 1.x.x
```

---

### **Step 2: Set Up a New Poetry Project**

#### 2.1. **Create a New Project with Poetry**

To create a new Python project, navigate to the directory where you want the project to reside and run the following command:

```bash
poetry new my_project
```

This will create a new directory `my_project` with a basic project structure, including a `pyproject.toml` file which is used to manage the project’s dependencies and other settings.

For example, the created project structure will look like:

```
my_project/
    ├── my_project/
    │   └── __init__.py
    ├── pyproject.toml
    └── tests/
        └── __init__.py
```

- `pyproject.toml`: This is the main configuration file for the project.
- `my_project/`: Contains your main Python package.
- `tests/`: Contains default test files.

#### 2.2. **Understanding `pyproject.toml`**

The `pyproject.toml` file defines the project’s metadata and dependencies. For instance, it might look like this:

```toml
[tool.poetry]
name = "my_project"
version = "0.1.0"
description = "A simple Python project."
authors = ["Your Name <you@example.com>"]

[tool.poetry.dependencies]
python = "^3.8"

[tool.poetry.dev-dependencies]
pytest = "^6.2"
```

- `[tool.poetry]`: Metadata about your project, such as its name, version, and author.
- `[tool.poetry.dependencies]`: Lists the runtime dependencies for your project.
- `[tool.poetry.dev-dependencies]`: Lists the development dependencies, such as testing frameworks.

---

### **Step 3: Install Project Dependencies**

#### 3.1. **Activate the Virtual Environment**

Poetry automatically creates and manages a virtual environment for your project. You can install your dependencies by running:

```bash
cd my_project  # Navigate into your project directory
poetry install  # Install all dependencies listed in pyproject.toml
```

Poetry will create a virtual environment specifically for this project, separate from the global Python environment. It will also install any dependencies defined in the `pyproject.toml` file.

#### 3.2. **Activate the Virtual Environment Manually (Optional)**

You can activate the virtual environment manually with the following command:

```bash
poetry shell
```

This activates the Poetry-managed virtual environment, and you'll be able to run Python commands and install additional dependencies in the isolated environment.

#### 3.3. **Add New Dependencies**

If you need to add new dependencies, you can use the `poetry add` command. For example:

```bash
poetry add requests
```

This will install the `requests` library and update your `pyproject.toml` and `poetry.lock` files automatically.

To add a development dependency (e.g., `pytest`):

```bash
poetry add --dev pytest
```

#### 3.4. **Remove Dependencies**

If you want to remove a dependency, use:

```bash
poetry remove requests
```

This will uninstall `requests` and update your `pyproject.toml` and `poetry.lock` files.

---

### **Step 4: Use the Virtual Environment for Development**

#### 4.1. **Start Coding**

Once the environment is set up and dependencies are installed, you can start developing your project. Your Python code will reside in the `my_project/` directory, and you can write code in files like `my_project/my_project.py`.

For example, you might edit `my_project/my_project.py` to include the following:

```python
def greet(name):
    return f"Hello, {name}!"
```

#### 4.2. **Run Python Code from the Virtual Environment**

To run your Python code while the virtual environment is active, you can simply use the Python interpreter:

```bash
python my_project/my_project.py
```

Alternatively, if you have installed additional dependencies (e.g., `requests`), you can import and use them in your code as needed.

#### 4.3. **Run Tests (Optional)**

If you have test cases (e.g., inside the `tests/` folder), you can use `pytest` to run them:

```bash
poetry run pytest
```

This runs your tests within the Poetry-managed environment, ensuring that any dependencies required for testing are available.

---

### **Step 5: Lock and Publish Your Project (Optional)**

#### 5.1. **Locking Dependencies**

Poetry automatically generates a `poetry.lock` file that records the exact versions of dependencies installed. This file ensures that the same versions of dependencies are installed across all environments (e.g., other developers or production).

You can manually update the `poetry.lock` file using:

```bash
poetry lock
```

#### 5.2. **Publishing a Package (Optional)**

If you want to publish your project to the Python Package Index (PyPI), you can use the following commands:

1. **Build the package**:

    ```bash
    poetry build
    ```

2. **Publish the package**:

    ```bash
    poetry publish --build
    ```

You’ll need to authenticate with your PyPI credentials when publishing.

---

### **Step 6: Managing Multiple Projects or Virtual Environments**

Poetry allows you to manage multiple Python projects without conflicts. Each project will have its own virtual environment, making it easy to switch between them.

To manage environments, you can use:

- **Activate environment for the current project**:

    ```bash
    poetry shell
    ```

- **Deactivate the environment**:

    Simply type `exit` or close the terminal.

---

### **Additional Poetry Commands**

Here are a few other useful commands:

- **Check the status of dependencies**:

    ```bash
    poetry show
    ```

- **Get help on any command**:

    ```bash
    poetry help <command>
    ```

- **Update dependencies**:

    ```bash
    poetry update
    ```

- **Show detailed dependency tree**:

    ```bash
    poetry show --tree
    ```

---

#### 1.4. **Install Dependencies (Optional)**

If you want to add external dependencies for your package, you can use:

```bash
poetry add <dependency-name>
```

For example, if you wanted to add `requests` as a dependency:

```bash
poetry add requests
```

---

### **Step 2: Build and Publish the Python Package**

#### 2.1. **Build the Package**

Once your library is ready, you can build it into a distributable format. Run the following command:

```bash
poetry build
```

This will create distribution archives (usually `.tar.gz` and `.whl` files) in the `dist/` folder.

#### 2.3. **Local Installation (Optional)**

If you don’t want to publish the library to PyPI yet but want to install it locally, you can install the package directly from your local directory. In the `my_library` directory, run:

```bash
poetry install
```

This will install the package in your local Poetry environment.

---

### **Step 3: Use Your Package in Another Project**

Once the library is ready, you can use it in another project by either publishing it to PyPI or installing it locally.

#### 3.1. **Create Another Project Using Poetry**

In a new directory, you can create another project:

```bash
poetry new my_app
cd my_app
```

This will create the `my_app` project with a basic structure, similar to what you did in the previous steps.

#### 3.2. **Add Your Library as a Dependency**

##### **Option 2: Install from a Local Directory (Without Publishing)**

If you want to install the library locally without publishing it to PyPI, you can add it as a dependency in the `pyproject.toml` file.

For example, if the `my_library` package is located in `/path/to/my_library`, you can run:

```bash
poetry add ../path/to/my_library
```

Alternatively, you can manually edit the `pyproject.toml` to include the relative path to your local package:

```toml
[tool.poetry.dependencies]
my_library = { path = "../path/to/my_library" }
```

After that, run:

```bash
poetry install
```

Poetry will install the local version of `my_library` from the specified path.

#### 3.3. **Using the Library in Your Code**

Now you can use `my_library` in your project code. For example, edit `my_app/my_app.py` to use the `greet` function from `my_library`:

```python
# my_app/my_app.py

from my_library.my_library import greet

def main():
    print(greet("World"))

if __name__ == "__main__":
    main()
```

You can then run your project:

```bash
python my_app/my_app.py
```

This should output:

```
Hello, World!
```

---

### **Step 4: Development with Your Library**

During development, if you're actively working on `my_library`, you can install it in "editable" mode in your new project to reflect changes immediately.

In the `my_app` project directory, you can install the library in editable mode by running:

```bash
poetry add ../path/to/my_library --editable
```

This way, changes to the code in `my_library` will immediately be reflected in your `my_app` project without needing to reinstall the package each time.

---
