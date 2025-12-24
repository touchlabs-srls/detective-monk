# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  config.vm.box = "bento/ubuntu-24.04"
  #config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.hostname = "monk"
  config.vm.synced_folder "input", "/opt/monk/input"
  config.vm.synced_folder "output", "/opt/monk/output"
  config.vm.synced_folder "bin", "/usr/local/sbin"

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
    apt-get install -y ca-certificates fonts-liberation libasound2-dev libatk-bridge2.0-0 libatk1.0-0 libc6 libcairo2 \
        libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgbm1 libgcc1 libglib2.0-0 libgtk-3-0 libnspr4 libnss3 \
        libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 \
        libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 lsb-release wget xdg-utils

    # Utilities
    apt-get install -y lnav silversearcher-ag git curl

    # Node 20
    mkdir -p /etc/apt/keyrings
    curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
    NODE_MAJOR=20
    echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
    apt-get update
    apt-get install -y nodejs

    # Set cache directory to the home of Vagrant user
    export PUPPETEER_CACHE_DIR="/home/vagrant/.cache/puppeteer"

    # EDPS Website Evidence Collector 2.1.2
    npm install -g https://code.europa.eu/EDPS/website-evidence-collector/-/archive/v2.1.2/website-evidence-collector-v2.1.2.tar.gz

    # # OVH Website Evidence Collector Batch
    git clone https://github.com/ovh/website-evidence-collector-batch.git /opt/monk/website-evidence-collector-batch
    cd /opt/monk/website-evidence-collector-batch && npm install && npm link

    # Extend timeout (needed for some websites)
    TIMEOUT=90000
    sed -s -i "s/30000/$TIMEOUT/g" /opt/monk/website-evidence-collector-batch/src/index.js
    sed -s -i "s/30000/$TIMEOUT/g" /opt/monk/website-evidence-collector-batch/src/lib/urls.js

    # Ensure Chrome is installed
    /usr/lib/node_modules/website-evidence-collector/node_modules/.bin/puppeteer browsers install chrome
    chown -R vagrant:vagrant /home/vagrant/.cache

    # Install sitemap tools
    npm install -g sitemap-generator-cli
    apt-get install -y python3-pip
    pip install scrape-cli --break-system-packages

    echo ""
    echo "Monk VM provisioning completed!"
  SHELL
end
