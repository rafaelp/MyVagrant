# My Vagrant

The purpose of this project is to setup one vagrant box with basic setup of everything you need to develop a Rails application using PostgreSQL.

The ideia is serve as a base if you want to configure your own vagrant box with requirements you need.

So, feel free to copy and modify as your need.

## Pre-requisites

1. [Vagrant](http://docs.vagrantup.com/v2/installation/)

1. [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

1. [RVM](http://rvm.io/rvm/install) or [rbenv](https://github.com/sstephenson/rbenv#installation) or [Ruby](https://www.ruby-lang.org/en/downloads/)

1. [Rubygems](https://rubygems.org/pages/download)

## Setup

1. Clone the project to your machine:

		$ git clone git@github.com:rafaelp/MyVagrant.git && cd MyVagrant

1. Install bundler:

		$ gem install bundler

1. Install required gems:

		$ bundle install

    This will basically install [Librarian-Chef](https://github.com/applicationsonline/librarian-chef).

1. Install cookbooks:

		$ librarian-chef install

1. Run Vagrant (this will take up to 10 minutes at first time)

		$ vagrant up

    Attention: You should not be running postgresql on your machine.

1. Be Happy!

  At this point you can connect to the server using:

		$ vagrant ssh

  and connect to postgresql as it was installed on your machine, because the box is configure to forward the port 5432

		$ psql -U postgres

  to stop the machine use:

		$ vagrant halt

  and to start again:

		$ vagrant up


## Ruby on Rails

You can use the files Cheffile and Vagrantfile inside your Ruby on Rails project.
Just add the following configuration on Vagrantfile

```ruby
Vagrant::Config.run do |config|

  ...

  config.vm.forward_port 3000, 3000

  ...

  # Run the Rails project right on vagrant up
  config.vm.provision :shell, inline: %q{cd /vagrant && export DISPLAY=:99}
  config.vm.provision :shell, inline: %q{sudo /etc/init.d/xvfb start}
  config.vm.provision :shell, inline: %q{sudo bash -c "echo 'UseDNS no' >> /etc/ssh/sshd_config"}
  config.vm.provision :shell, inline: %q{cp /vagrant/config/database.sample.yml /vagrant/config/database.yml}
  config.vm.provision :shell, inline: %q{cd /vagrant && bundle install}
  config.vm.provision :shell, inline: %q{cd /vagrant && rake db:create db:migrate db:test:prepare db:seed}
  config.vm.provision :shell, inline: %q{cd /vagrant && rails s -d}

end
```

or, if you use [foreman](https://github.com/ddollar/foreman):

```ruby
Vagrant::Config.run do |config|

  ...

  config.vm.forward_port 5000, 5000

  ...

  # Run the Rails project right on vagrant up
  config.vm.provision :shell, inline: %q{cd /vagrant && export DISPLAY=:99}
  config.vm.provision :shell, inline: %q{sudo /etc/init.d/xvfb start}
  config.vm.provision :shell, inline: %q{sudo bash -c "echo 'UseDNS no' >> /etc/ssh/sshd_config"}
  config.vm.provision :shell, inline: %q{cp /vagrant/config/database.sample.yml /vagrant/config/database.yml}
  config.vm.provision :shell, inline: %q{cd /vagrant && bundle install}
  config.vm.provision :shell, inline: %q{cd /vagrant && rake db:create db:migrate db:test:prepare db:seed}
  config.vm.provision :shell, inline: %q{cd /vagrant && foreman start}

end
```

## To-do

Don't you want to play with that and make a pull-request!?

1. Add cookbook for mongodb

## Issues

If you have problems, please create a [Github Issue](https://github.com/rafaelp/MyVagrant/issues).

## Contributing

Please see [CONTRIBUTING.md](https://github.com/rafaelp/MyVagrant/blob/master/CONTRIBUTING.md) for details.

## Credits

MyVagrant was originally written by [Rafael Lima](http://rafael.adm.br).
Thank you to all the [contributors](https://github.com/rafaelp/MyVagrant/graphs/contributors).

## License

MyVagrant is Copyright © 2014 Rafael Lima. It is free software, and may be redistributed under the terms specified in the [LICENSE](https://github.com/rafaelp/MyVagrant/blob/master/LICENSE) file.