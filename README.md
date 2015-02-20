# Vagrant Tips & Hints #

## Introduction ##

At [Weaveworks](http://weave.works) we use [Vagrant](http://vagrantup.com) for a number of
our [getting started examples](https://github.com/fintanr/weave-gs). These notes are a small scratchpad of useful plugins,
tips and hacks I have come across. 

## Useful Plugins ##

### Vagrant Hostname ###

[Vagrant Hostmanager](https://github.com/smdahlen/vagrant-hostmanager) - manage /etc/hosts entries on guests(s) and hosts  

```
 vagrant plugin install vagrant-hostmanager
```

Basic details for a Vagrantfile
```
if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
end
```
 
### Vagrant Cachier ###

[Vagrant Cachier](https://github.com/fgrehm/vagrant-cachier) - shares a common package cache among similar VM instances.

```
vagrant plugin install vagrant-cachier
```

Basic details for a Vagrantfile
```
if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
end
```

## Code Snippets ##

Increment IP address, .e.g for private_network interfaces. 

For some reason this comes up a lot as a question and gets a lot of hacks as answers. 
Ruby has [ipaddr](http://ruby-doc.org//stdlib-1.9.3/libdoc/ipaddr/rdoc/IPAddr.html) as a standard libary and it deals with anything you are likely to throw at it.

```
require 'ipaddr'

$starting_ip = "172.17.8.100"

ip = IPAddr.new($vm_ip)
$vm_ip = ip.succ.to_s
config.vm.network "private_network", ip: $vm_ip
```
