# Detective Monk
#### A Vagrant wrapper for website-evidence-collector-batch

Detective Monk is a simple wrapper that loads a [Vagrant][Vagrant] box with
[website-evidence-collector-batch][wecb]. It includes a custom script to 
automate loading configuration files and a utility to generate configuration
files by scraping websites and creating sitemaps.

## Features

- Install and set up required tools
- Import configuration from `input/config.yml`
- Read cookies from `input/cookies.txt`
- Write the output in a sub-directory of `output/`
- Generate configuration files by scraping websites
- Automatically discover URLs for scanning

## Tech

Detective Monk uses the following open source projects:

- [Vagrant] - Automate the set up of the virtual machine
- [website-evidence-collector][wec] - Scan websites for cookie usage and
  external resources
- [website-evidence-collector-batch][wecb] - Automate scans on URL lists or
  entire `sitemap.xml` files
- [sitemap-generator-cli] - Generate sitemaps from websites
- [scrape-cli] - Extract data from XML/HTML using XPath expressions

## Installation

This project requires [Vagrant] to run, and a virtualization provider such as
[VirtualBox] or [VMware].

You can spin up the virtual machine by running:

```sh
vagrant up
```

## Usage

First, copy the `input/config.example.yml` to `input/config.yml` and adjust it
to your needs. The configuration file follows the
[website-evidence-collector-batch][wecb] format.

If you need to set up a cookie jar, add it to `input/cookies.txt`.

Finally, run the collection by calling the `monk` wrapper script.

```sh
vagrant ssh -c monk
```

You will find the results in the `output/` directory.

### Configuration File Generator

You can also generate a configuration file by creating a sitemap using the
`monk-sitemap-generator` script. This is useful when the website you want
to scan does not provide a sitemap.

```sh
vagrant ssh -c "monk-sitemap-generator https://example.com"
```

This will temporarily generate a sitemap for the specified website and use it
to create a configuration file in `input/config-example.com.yml`.

You can review the generated file and copy it to `config.yml` when satisfied
with the result.

The virtual machine can be stopped with:

```sh
vagrant halt
```

It can be destroyed with:

```sh
vagrant destroy
```


  [Vagrant]: <https://www.vagrantup.com/>
  [wec]: https://github.com/EU-EDPS/website-evidence-collector
  [wecb]: <https://github.com/ovh/website-evidence-collector-batch>
  [sitemap-generator-cli]: <https://www.npmjs.com/package/sitemap-generator-cli>
  [scrape-cli]: <https://github.com/aborruso/scrape-cli>
  [VirtualBox]: <https://www.virtualbox.org/>
  [VMware]: https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion
