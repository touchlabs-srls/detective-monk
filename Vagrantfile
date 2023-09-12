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
    # https://github.com/puppeteer/puppeteer/blob/main/docs/troubleshooting.md
    apt-get update
    apt-get install -y ca-certificates fonts-liberation libasound2 libatk-bridge2.0-0 libatk1.0-0 libc6 libcairo2 \
        libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgbm1 libgcc1 libglib2.0-0 libgtk-3-0 libnspr4 libnss3 \
        libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 \
        libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 lsb-release wget xdg-utils

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
