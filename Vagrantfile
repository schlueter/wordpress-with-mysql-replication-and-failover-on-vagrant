# -*- mode: ruby -*-
# vi: set ft=ruby :

##
#    This Vagrantfile provisions a web server hosting wordpress served
#    using Nginx and a master slave configuration of percona sql servers.
#
#    Copyright (C) 2015 Brandon Schlueter
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License
#    along with this program.  If not, see <http://www.gnu.org/licenses/>.
##

Vagrant.configure(2) do |config|
  FAILOVER_FILE_PREFIX = <<-EOF
This file is generated by the Vagrantfile in this directory.
It states the current sql master on a line starting with 'master='.
If that line or this file does not exist, the Vagrantfile will
assume the 'sql1' box is the master. Any other line is ignored.

  EOF

  IP_ADDRESSES = {
    database_1: '192.168.10.11',
    database_2: '192.168.10.12',
    webserver: '192.168.10.13'
  }

  config.vm.box = 'ubuntu/trusty64'
  config.vm.define 'web' do |web|
    web.vm.network :private_network, ip: IP_ADDRESSES['webserver']

    web.vm.provision :chef_solo do |chef|
      chef.run_list = %w(failover-wordpress::wordpress)
      chef.json = {
        failover_wordpress: {
          database: {
            master: {
              host: IP_ADDRESSES['database_1']
            }
          }
        }
      }
    end
  end

  def failover_provision(config, master)
    if master
      config.vm.provision :chef_solo do |_|
      end
    else
      config.vm.provision :chef_solo do |_|
      end
    end
  end

  def sql1(config, failover=false, master=true)
    config.vm.define 'sql1' do |sql|
      sql.vm.network :private_network, ip: IP_ADDRESSES['database_1']

      if failover
        failover_provision(sql, master)
      else
        sql.vm.provision :chef_solo do |_|
        end
      end
    end
  end

  def sql2(config, failover=false, master=false)
    config.vm.define 'sql2' do |sql|
      sql.vm.network :private_network, ip: IP_ADDRESSES['database_2']

      if failover
        failover_provision(sql, master)
      else
        sql.vm.provision :chef_solo do |_|
        end
      end
    end
  end

  if ENV['FAILOVER']
    master = nil

    begin
      # Read current master from .failover file
      ::File.open('.failover', 'r').each do |line|
        master = line.split('=')[1] if line.start_with? 'master='
      end
    rescue Errno::ENOENT
      puts 'No .failover file found. Assuming sql1 is current master'
    end

    # Default to sql1
    master ||= 'sql1'

    def populate_failover_file(master)
      # Ruby doesn't have a good way to modify a file in-place, and this was easiest, and safe
      # Clear the contents of the file
      File.open('.failover', 'w') {}
      # Populate the file with the current master
      ::File.open('.failover', 'w') do |file|
        file.write FAILOVER_FILE_PREFIX
        file.write "master=#{master}"
      end
    end

    if master == 'sql2'
      puts 'sql2'
      populate_failover_file 'sql1'
    else
      puts 'sql1'
      populate_failover_file 'sql2'
    end
  else
    sql1 config
    sql2 config
  end
end
