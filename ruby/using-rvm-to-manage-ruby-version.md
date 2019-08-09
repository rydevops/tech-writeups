# Using RMV to manage Ruby version

## Installation
The following documentation outlines the process to install and active RVM's current stable
version including the latest stable version of Ruby. These instructions outline the process
on Mac OS X however most of this should translate to any Nix derivative. 

1.  Install the latest version of gnu pg using brew
    ```bash
    $ brew install gnupg
    ```
2.  Install the required gpg key. Key can be found by visiting https://rvm.io/rvm/install.
    ```shell
    $ gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
    ```
3.  Install RVM (again latest version including latest Ruby version)
    ```
    \curl -sSL https://get.rvm.io | bash -s stable --ruby
    ```
4.  Once installed relaunch the shell


## Commands

`rvm list` - List installed versions including the current and default version<br>
`rvm list known` - List known and available Ruby versions that can be installed<br>
`rvm install ruby-2.7` - Install specific version of Ruby<br>
`rvm use ruby-2.6.3` - Change the current active version
