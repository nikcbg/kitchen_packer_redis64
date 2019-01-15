# kitchen_packer_redis64

### Purpose of the repository 
- The repository creates `vagrant` virtualbox box with `redis64` on it and test it with `kitchen` framework. 

#### List of files in the repository

File name | File description
----------|----------------------------------
`http/preseed.cfg` | script that installs Ubuntu.
`scripts/provision.sh` | script that installs `redis-server`.
`template.json` | template with code for `packer` toll to create `vagrant` virtualbox box. 
`test/integration/default/check_pkg.rb` | script needed by `kitchen` to check if `redis-server` is installed. 
`.gitignore` | which files and directories to ignore.
`.kitchen.yml` | configuration file for `kitchen` test framework.
`Gemfile` | used for `ruby` dependencies.

### How to use this repository.
- Install `virtualbox` by following this [instructions](https://www.virtualbox.org/wiki/Downloads).
- Install `vagrant` by following this [instructions](https://www.vagrantup.com/docs/installation/).
- Install `packer` by following this [instructions](https://www.packer.io/intro/getting-started/install.html).
- Clone the repository to your local computer: `git clone git@github.com:nikcbg/kitchen_packer_redis64.git`.
- Go to the cloned repo on your computer: `cd kitchen_packer_redis64`.
- Execute `packer validate template.json` to validates `template.json` file, after executing the command it should return `Template validated successfully` message. 
- Execute `packer build template.json`  to start building the virtual machine you need to run your tests on. 
- After that you should see this message `redis64_2-vbox: 'virtualbox' provider box: redis64_2-vbox.box` which means that the VM box was created successfully.
- Execute `vagrant box list` to see the list of `vagrant` boxes.
- Execute `vagrant box add --name redis64 redis64-vbox.box` to add the newly created `packer` box. 
- Execute `vagrant init redis64` to create Vagrantfile if one doesn't already exist.  


### Setting up `ruby` environment on Ubuntu 18.04 instructions:
- Before testing with `kitchen` you need to install and prepare `ruby` environment.
- Execute `sudo apt update` to update the packages on your Ubuntu computer. 
- Execute `sudo apt install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm5 libgdbm-dev` to install `ruby` dependencies.
- Execute `wget -q https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-installer -O- | bash` to download and install `rbenv` environment
- Execute `echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile` to change your `~/.bashrc` file to use `ruby` command line utility 
- Execute `echo 'eval "$(rbenv init -)"' >> ~/.bashrc` so `rbenv` loads automatically.
- Execute `source ~/.bash_profile` to reload `bash` profile.
- Execute `type rbenv` command to verify that `rbenv` is set up properly, the output will display the following:
```
rbenv is a function
rbenv ()
{
    local command;
    command="${1:-}";
    if [ "$#" -gt 0 ]; then
        shift;
    fi;
    case "$command" in
        rehash | shell)
            eval "$(rbenv "sh-$command" "$@")"
        ;;
        *)
            command rbenv "$command" "$@"
        ;;
    esac
}
```

- Execute `rbenv install 2.5.3` to install `ruby 2.5.3` version.
- Execute `rbenv local 2.5.3` to set the default version of `ruby` to your local directory.
- Execute `rbenv -v` to make sure `ruby` is installed and you have the correct version.
- Execute `gem install bundler` to install `gem` which is package manager for `ruby`, the output will display the following:
```
Successfully installed bundler-1.17.1
1 gem installed
```

### Commands needed to test with `kitchen`.

Command execution | Command outcome
------------------|-------------------------------
`bundle exec kitchen list` | to list `kitchen` instances.
`bundle exec kitchen converge` | to create `kitchen` environment.
`bundle exec kitchen verify` | command to execute `kitchen` test.
`bundle exec kitchen destroy` | to destroy `kitchen` environment.
`bundle exec kitchen test` | to automatically build, test and destroy `kitchen` environment.

### TO DO:
- Check if `redis server` is installed and running. 
