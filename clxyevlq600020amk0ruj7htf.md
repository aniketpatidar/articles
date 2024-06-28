---
title: "Setting Up asdf to Manage Node.js and Ruby on Ubuntu"
datePublished: Fri Jun 28 2024 08:08:22 GMT+0000 (Coordinated Universal Time)
cuid: clxyevlq600020amk0ruj7htf
slug: setting-up-asdf-to-manage-nodejs-and-ruby-on-ubuntu
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1719562041276/3f8db288-2cc2-41b5-aaad-fac9222429ef.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1719562092510/f98f4549-5d0d-4c43-8e4d-5dcbf2227ae1.png

---

Managing multiple versions of programming languages can be a headache, but asdf simplifies this process by providing a single CLI tool to manage versions of multiple runtime languages. In this article, we'll walk through the steps to install and configure asdf on Ubuntu, along with Node.js and Ruby.

## Prerequisites

Before we begin, ensure that you have `curl` and `git` installed on your system. You can install them using the following command:

```bash
sudo apt install curl git
```

## Step 1: Clone the asdf Repository

First, clone the asdf repository from GitHub into your home directory:

```bash
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.14.0
```

This command checks out the version 0.14.0 of asdf. Adjust the version number as needed.

## Step 2: Update Your Shell Configuration

To make asdf available in your terminal, you need to update your shell configuration. Add the following lines to your `~/.bashrc` file:

```bash
. "$HOME/.asdf/asdf.sh"
. "$HOME/.asdf/completions/asdf.bash"
```

After adding these lines, reload your `~/.bashrc` file to apply the changes:

```bash
source ~/.bashrc
```

## Step 3: Install Node.js

To install Node.js using asdf, follow these steps:

### Install Additional Dependencies

Node.js requires some additional dependencies which can be installed using the following command:

```bash
sudo apt-get install dirmngr gpg curl gawk
```

### Add the Node.js Plugin

Next, add the Node.js plugin to asdf:

```bash
asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git
```

### Install the Latest Version of Node.js

You can now install the latest version of Node.js:

```bash
asdf install nodejs latest
```

## Step 4: Install Ruby

To install Ruby using asdf, follow these steps:

### Add the Ruby Plugin

First, add the Ruby plugin to asdf:

```bash
asdf plugin add ruby https://github.com/asdf-vm/asdf-ruby.git
```

### Install the Latest Version of Ruby

Now, you can install the latest version of Ruby:

```bash
asdf install ruby latest
```

## Step 5: Verify the Installation

It's important to verify that asdf has correctly installed and configured the runtime versions. You can do this by checking the output of the `type -a ruby` command.

* **Correct Output:**
    
    ```bash
    ruby is /home/username/.asdf/shims/ruby
    ruby is /usr/bin/ruby
    ```
    
* **Incorrect Output:**
    
    ```bash
    ruby is /usr/bin/ruby
    ```
    

If you see the correct output, it means that asdf is correctly managing the Ruby version.

## Conclusion

By following these steps, you've installed asdf and configured it to manage Node.js and Ruby on your system. This setup will help you seamlessly switch between different versions of these runtimes, making your development environment more flexible and efficient.

Happy coding!

---

Feel free to ask any questions in the comments below or share your experience using asdf to manage different runtime versions. If you found this article helpful, don't forget to give it a thumbs up and share it with your network!