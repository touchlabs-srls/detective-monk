# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  config.vm.box = "bento/ubuntu-20.04"
  #config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.hostname = "monk"
  config.vm.synced_folder "input", "/opt/monk/input"
  config.vm.synced_folder "output", "/opt/monk/output"
  config.vm.synced_folder "bin", "/usr/local/bin"

  config.vm.provision "shell", inline: <<-SHELL
    # Prepare environment
    mkdir -p /opt/monk/input
    mkdir -p /opt/monk/output

    export DEBIAN_FRONTEND=noninteractive

    # Configure timezone
    cp /usr/share/zoneinfo/Europe/Rome /etc/localtime

    # Needed libraries
    # https://gist.github.com/winuxue/cfef08e2f5fe9dfc16a1d67a4ad38a01?permalink_comment_id=4116618#gistcomment-4116618
    apt-get update
    apt-get install -y apt install -y libx11-xcb1 libxcomposite1 libxcursor1 libxdamage1 libxi-dev libxtst-dev libnss3 \
        libcups2 libxss1 libxrandr2 libasound2 libatk1.0-0 libatk-bridge2.0-0 libpangocairo-1.0-0 libgtk-3-0 libgbm1

    # Utilities
    apt-get install -y lnav silversearcher-ag git curl

    # Node 16
    curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
    apt-get install -y nodejs

    # EDPS Website Evidence Collector
    npm install -g https://github.com/EU-EDPS/website-evidence-collector/tarball/latest
    # Fix links
    ln -s /usr/lib/node_modules/website-evidence-collector/assets /usr/lib/node_modules/website-evidence-collector/bin/assets
    ln -s /usr/lib/node_modules/website-evidence-collector/assets/wec_logo.svg /usr/lib/node_modules/website-evidence-collector/bin/wec_logo.svg

    # OVH Website Evidence Collector Batch
    git clone https://github.com/ovh/website-evidence-collector-batch.git /opt/monk/website-evidence-collector-batch
    cd /opt/monk/website-evidence-collector-batch && npm install && npm link
  SHELL
end
