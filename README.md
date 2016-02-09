# Project reference setup with ansible & vagrant

[![Build Status](https://travis-ci.org/wunderkraut/WunderMachina.svg?branch=centos7)](https://travis-ci.org/wunderkraut/WunderMachina)

##Setup

You can setup the IP address and the url of your vagrant machine on the
Vagrantfile.

Edit the variables.yml file to setup the URL and dir for the docs of your project.

Remember to add also the entries in your hosts file for both the drupal install and
the docs.


###
Requirements:
- Vagrant 1.5.x
- https://github.com/fgrehm/vagrant-cachier
( $ vagrant plugin install vagrant-cachier )
- Ansible in your host machine. For OS X:
 brew install ansible

##Introduction

Start by running:

  $ vagrant up

This will do the following:

- clone the latest WunderMachina ansible/vagrant setup (or the version specified in conf/project.yml)
- Bring up & provision the virtual machine (if needed)
- Build the drupal site under drupal/current (not yet actually)

After finishing provisioning (first time is always slow) and building the site
you need to install the Drupal site in http://x.x.x.x:8080/install.php
(Note: on rare occasion php-fpm/varnish/e.g. requires to be restarted before
starting to work. You can do this by issuing the following command:

  $ vagrant  ssh -c "sudo service php-fpm restart"
  $ vagrant  ssh -c "sudo service varnish restart"

All Drupal related configurations are under drupal/conf

Drush is usable without ssh access with the drush.sh script e.g:

  $ ./drush.sh cc all

To open up ssh access to the virtual machine:

  $ vagrant ssh


-------------------------------------------------------------------------------
Useful things

At the moment IP is configured in
  Vagrantfile
    variable INSTANCE_IP

Varnish responds to
  http://x.x.x.x/

Nginx responds to
  http://x.x.x.x:8080/

Solr responds to
  http://x.x.x.x:8983/solr

MailHOG responds to
  http://x.x.x.x:8025

Docs are in
        http://x.x.x.x:8080/index.html
        You can setup the dir where the docs are taken from and their URL from the
        variables.yml file.

        #Docs
        docs:
          hostname : 'docs.local.ansibleref.com'
          dir : '/vagrant/docs'


##Vagrant + Ansible configuration

Vagrant is using Ansible provision stored under the ansible subdirectory.
The inventory file (which stores the hosts and their IP's) is located under
ansible/inventory. Host specific configurations for Vagrant are stored in
ansible/vagrant.yml and the playbooks are under ansible/playbook directory.
Variable overrides are defined in ansible/variables.yml.

You should only bother with the following:

  Vagrant box setup
    conf/vagrant.yml

  What components do you want to install?
    conf/vagrant.yml

  And how are those set up?
    conf/variables.yml

You can also fix your vagrant/ansible base setup to certain branch/revision
    conf/project.yml
  There you can also do the same for build.sh



## Debugging tools

XDebug tools are installed via the devtools role. Everything should work out
of the box for PHPStorm. PHP script e.g. drush debugging should also work.

Example sublime text project configuration (via Project->Edit Project):

    {
       "folders":
       [
         {
           "follow_symlinks": true,
           "path": "/path/to/ansibleref"
         }
       ],

       "settings":
       {
         "xdebug": {
              "path_mapping": {
                    "/vagrant" : "/path/to/ansibleref"
                 }
            }
          }
    }

