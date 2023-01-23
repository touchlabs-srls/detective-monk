# Detective Monk
#### A Vagrant wrapper for website-evidence-collector-batch

Detective Monk is a simple wrapper that loads a [Vagrant][Vagrant] box with [website-evidence-collector-batch][wecb],
adding a custom script to automate loading the configuration file.

## Features

- Install and set up required tools
- Import configuration from `input/config.yml`
- Read cookies from `input/cookies.txt`
- Write the output in a sub-directory of `output/`

## Tech

Detective Monk uses the following open source projects:

- [Vagrant] - Automate the set up of the virtual machine
- [website-evidence-collector][wec] - Scan a website for the use of cookies and external resources
- [website-evidence-collector-batch][wecb] - Automate scans on a list of URLs or entire `sitemap.xml` files

## Installation

This project requires [Vagrant] to run, and a virtualization provider such as [VirtualBox].

You can spin up the virtual machine by running:

```sh
vagrant up
```

## Usage

First, copy the `input/config.example.yml` to `input/config.yml` and adjust it to your needs. The configuration file
follows the [website-evidence-collector-batch][wecb] format.

If you need to set up a cookie jar, add it to `input/cookies.txt`.

Finally, run the collection by calling the  `monk` wrapper script.

```sh
vagrant ssh -c monk
```

You will find the results in the `output/` directory.

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
   [VirtualBox]: <https://www.virtualbox.org/>
