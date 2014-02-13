# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$script = <<SCRIPT
:
: install
(
    yum install -y nscd libnss-mysql mysql mysql-server
    /etc/init.d/mysqld status >/dev/null || /etc/init.d/mysqld start

    mysql -uroot -Dauth 2>/dev/null || ( 
        mysql -uroot < /usr/share/doc/libnss-mysql-1.5/sample/linux/sample_database.sql
        echo 'INSERT INTO groups    (name, gid)    VALUES ("hogehoge", 5010)' | mysql -uroot -Dauth 
        echo 'INSERT INTO grouplist (gid, username) VALUES (5010, "cinergi")' | mysql -uroot -Dauth
    )

    perl -pi'' -e 's/passwd:     files$/passwd:     compat mysql/' /etc/nsswitch.conf
    perl -pi'' -e 's/shadow:     files$/shadow:     compat mysql/' /etc/nsswitch.conf
    perl -pi'' -e 's/group:      files$/group:      compat mysql/' /etc/nsswitch.conf

    # nscd should start after modifying /etc/nsswitch.conf
    /etc/init.d/nscd status >/dev/null || /etc/init.d/nscd start

    id cinergi
    # `id cinergi` should print
    # uid=5000(cinergi) gid=5000(foobaz) groups=5000(foobaz),5010(hogehoge)
)
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box     = "CentOS 6.5 x86_64"
  config.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.5.1/centos65-x86_64-20131205.box"
  config.vm.provision "shell", inline: $script
end
