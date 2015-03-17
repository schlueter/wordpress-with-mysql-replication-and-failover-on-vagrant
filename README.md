# Wordpress using a Replicated database with easy fail over

This project is an example of a [Wordpress][1] site using a replicated database with single command fail over. This implementation runs [Virtual box][2] servers hosted via [Vagrant][3]. The default database is [MySQL][4] and Wordpress is served by [Nginx][5].

## Dependencies

- [Chef Development Kit][6]
[INVESTIGATE]: # (The space after 'Vagrant' below)
- [Vagrant ][3]
- [Vagrant Omnibus][7], obtainable by `vagrant plugin install vagrant-omnibus`
- [Vagrant Berkshelf][9], obtainable by `vagrant plugin install vagrant-berkshelf`

### Optional

- [Vagrant Cachier][8], obtainable by `vagrant plugin install vagrant-cachier`

## Usage

Run `vagrant up` in the root of this project.

### Failover

Fail over can be initiated by running `FAILOVER=1 vagrant provision` after the servers have been brought up. This command can be run repeatedly to switch the master database server. 

## License

Copyright (C) 2015 Brandon Schlueter
This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.
This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.
You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.


[1]: https://wordpress.org/
[2]: https://www.virtualbox.org/
[3]: https://www.vagrantup.com/
[4]: http://www.mysql.org/
[5]: http://nginx.org/
[6]: https://downloads.chef.io/chef-dk/
[7]: https://github.com/chef/vagrant-omnibus
[8]: http://fgrehm.viewdocs.io/vagrant-cachier
[9]: https://github.com/berkshelf/vagrant-berkshelf
