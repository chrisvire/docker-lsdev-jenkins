docker-lsdev-jenkins
=================

Jenkins environment to test changes to https://jenkins.lsdev.sil.org

Essentially, this project has two components; a Dockerfile configuring a main,
master Jenkins build server, and a Dockerfile configuring slave build
containers. The slave Dockerfile is located in a separate repository, at
[https://github.com/sillsdev/docker-appbuilder-agent](https://github.com/sillsdev/docker-appbuilder-agent).

This repository contains the master build server Dockerfile and
a [vagrant-boot2docker](https://github.com/silinternational/vagrant-boot2docker)
environment for local development and testing.

Vagrantfile
-----------

The include Vagrantfile spins up a boot2docker box, builds the docker image, 
and runs the `docker-compose` on the included docker-compose.yml to build and
start the Jenkins master and a slave agent.

After you have installed virtualbox and vagrant just run:

    vagrant up

The Jenkins master server will be accessible at
[http://192.168.70.221](http://192.168.70.221).

If you change the Dockerfile, you will need to run these commands:

    vagrant ssh
    cd /vagrant
    docker-compose stop
    docker-compose rm -f
    docker-compose build
    docker-compose up -d

Jenkins Configuration
---------------------

Configuration for a Jenkins server is stored within xml files on the local disk,
all beneath one directory know as the "Jenkins Home".

Plugins are installed as part of the Docker build process.  The complete list of
plugins (including dependencies) must be in the file build/plugins.txt

Any configuration changes from the base image are done using groovy scripts
during startup.  These groovy scripts are located in build/init.groovy.d.
See the docs at
[Configuring Jenkins upon start up](https://wiki.jenkins-ci.org/display/JENKINS/Configuring+Jenkins+upon+start+up)
and [Overview: Jenkins main module API](http://javadoc.jenkins-ci.org/).

