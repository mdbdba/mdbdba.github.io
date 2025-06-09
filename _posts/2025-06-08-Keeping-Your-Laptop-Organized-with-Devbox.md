---
tags:
  - Linux
  - Ubuntu
  - "24.04"
  - devbox
  - nix
  - development environment
  - isolated environments
  - dependency management
  - developer tools
  - environment isolation
  - software development
  - command line tools
  - project management
  - containerization
  - developer workflow
  - package management
  - open source tools
  - productivity tools
  - development setup
  - configuration management

---

# Keeping Your Laptop Organized with Devbox
Ever felt like your laptop is a cluttered desk drawer, with files scattered everywhere? You're not alone. As developers, 
we often juggle multiple projects, configurations, and dependencies that can quickly turn our systems into chaos.
Enter [**Devbox**](https://www.jetify.com/docs/devbox/) , your new best friend for keeping things tidy. Let's see how 
it can transform your development environment.

[**Devbox**](https://www.jetify.com/docs/devbox/) is an open source command line tool that allows us to easily create and 
manage isolated development environments. It provides a simple way to set up 
and manage development environments, making it easy to switch between different
projects and environments without having to worry about conflicts or dependencies.

## Why Devbox?
Imagine this: you start a new project on your laptop. You need to set up all the dependencies, tools, and 
configurations just right. But oh no, your current setup is full of old, incompatible packages from previous projects.
That's where **Devbox** shines. It provides standardized environments so that every project gets its own clean slate. 
No more conflicts between different projects' dependencies. It's like having a neat little cubbyhole for each project 
in your desk drawer.

## How Does It Work?
The magic of **Devbox** lies in its simplicity:
1. **Installation**: Get started with a simple command.
2. **Environment Creation**: Spin up isolated environments tailored to your specific project needs.
3. **Dependency Management**: Keep track of all dependencies effortlessly.

All from a JSON config file. 

That's really the value added by Devbox. It really is a bridge between
the gyrations and secret handshakes you need to learn if you want to use Nix, 
simplified to something mere mortals can use.

## Getting Started
### Installation
First, you install Devbox:
```console
    $ curl -fsSL https://get.jetify.com/devbox | bash
```
Now your system is ready to create isolated environments.

### Creating an Environment
`devbox init` creates the devbox.json file in the current directory with a default configuration.  
You would do this step if you are starting a new project.
```console
    $ mkdir /tmp/myCoolProject
    $ cd /tmp/myCoolProject 
    $ devbox init 
    
```
### Switching between projects
Since we are storing the configuration with our code, once we are in a directory 
with a devbox.json file, you can start a shell session with `devbox shell` and 
you can start working in that environment.

```console
    $ ls
    devbox.json
    
    $ devbox shell
    Info: Ensuring packages are installed.
    ✓ Computed the Devbox environment.
    Starting a devbox shell...
```
If you want to work in another project, you simply exit the current shell and switch to a new one by running 
`devbox shell` in another directory with a devbox.json file.
```console
    (devbox)
    $ exit

    $ cd ~/Git/go/tov_tools
    
    $ which go
    go not found
    
    $ devbox shell
    Starting a devbox shell...
    Welcome to tov_tools!
    ┄Tails of the Valiant Game tools.
    ...
    
    (devbox)
    $ which go
    ~/Git/go/tov_tools/.devbox/nix/profile/default/bin/go
```

### Managing Packages
With Devbox, you can easily add or remove packages without worrying about system-wide conflicts.
#### Adding a Package
```console
    (devbox)
    $ devbox add golangci-lint@2.1.6
    Info: Adding package "golangci-lint@2.1.6" to devbox.json
    Info: Installing the following packages to the nix store: golangci-lint@2.1.6
    ✓ Computed the Devbox environment.
    Warning: Your devbox environment may be out of date. Run refresh to update it.

    (devbox)
    $ refresh

```
#### Removing a Package
```console
    (devbox)
    $ devbox rm golangci-lint@2.1.6
    ✓ Computed the Devbox environment.
    Warning: Your devbox environment may be out of date. Run refresh to update it.

    (devbox)
    $ refresh
```
## Shared Configurations
With **Devbox**, you don't just keep your development environments organized; you also 
streamline collaboration. Share your Devbox configuration with your team as part of your code, 
and everyone can work in a consistent environment. 


## Conclusion: Why Bother?
**Devbox** isn't just a tool; it's your path to a stress-free development experience. By keeping dependencies isolated 
and environments consistent, you can focus on what matters—building awesome software.
So why wait? Give **Devbox** a try today and watch the chaos fade away.

I hope this gives you a good starting point for using Devbox to manage your development environments.
One of the great things about this tool is that its [documentation](https://www.jetify.com/docs/devbox/) is terrific.

