# -*- mode: ruby -*-
# vi: set ft=ruby :

# Specify Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

# Require 'yaml' module
require 'yaml'

# Read details of containers to be created from YAML file
# Be sure to edit 'containers.yml' to provide container details
containers = YAML.load_file('containers.yml')

# Create and configure the Docker container(s)
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Perform one-time configuration of Docker provider to specify
  # location of Vagrantfile for host VM; comment out this section
  # to use default boot2docker box
  config.vm.provider "docker" do |docker|
    docker.vagrant_vagrantfile = "DockerHostVagrantfile"
  end

  # Iterate through the entries in the YAML file
  containers.each do |container|
    config.vm.define container["name"] do |cntnr|

      # Disable synced folders for the Docker container
      # (prevents an NFS error on "vagrant up")
      cntnr.vm.synced_folder ".", "/vagrant", disabled: true

      # Configure the Docker provider for Vagrant
      cntnr.vm.provider "docker" do |docker|

        # Specify the Docker image to use, pull value from YAML file
        docker.image = container["image"]

        # Specify port mappings, pull value from YAML file
        # If omitted, no ports are mapped!
        docker.ports = container["ports"]

        # Specify a friendly name for the Docker container, pull from YAML file
        docker.name = container["name"]
      end
    end
  end
end
