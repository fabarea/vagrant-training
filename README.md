# Training environment Vagrant

A Vagrant file to set up a training server.

## Requirements

* Oracle VirtualBox
* Vagrant (of course)
* vagrant-hostmanager plugin to easily access the virtual machine using domain names
* Ruby (comes pre-installed on OS X)
* RubyGems - the Ruby package manager (also comes with OS X)
* Librarian-Chef to resolve dependencies of Chef cookbooks we use to provision the virtual machine
* Opscode Chef (knowledge optional but for sure nice to have)


## Setup

1. Download and install VirtualBox
2. Download and install Vagrant - the most recent version should be fine
3. Install the vagrant-hostmanager plugin: In a terminal execute `vagrant plugin install vagrant-hostmanager`
4. Use RubyGems to install Librarian Chef by executing the following command a terminal: `gem install librarian-chef`
5. Check out your project from source control
6. Let Librarian fetch all the Chef cookbooks and their dependencies by executing `librarian-chef install`
7. Now you are ready to `vagrant up` in your project directory


## Credits

Vagrant file inspired by https://github.com/irnnr/vagrant-typo3
