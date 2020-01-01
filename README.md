# Puppet rclocal module

[![Build Status](https://travis-ci.org/example42/puppet-rclocal.png?branch=master)](https://travis-ci.org/example42/puppet-rclocal)

#### Table of Contents
1. [Module Description](#module-description)
1. [Setup](#setup)
1. [Usage](#usage)
1. [Parameters](#parameters)
1. [Development](#development)
1. [Author](#author)

## Module description
This module manages rc.local entries in /etc/rc.local file directory.

## Setup

This module uses Data Types from puppetlabs-stdlib!

Systemd based systems will need [camptocamp/systemd](https://forge.puppet.com/camptocamp/systemd).

## Usage

To only manage the content of rc.local file:

    class { 'rclocal':
      # config_file - OS specific, set via hiera data in module
      # config_dir  - OS specific, set via hiera data in module
      # template    - name a template to fill rc.local file, defaults to module template
      # scripts     - a hash of rc.local.d snippets to add
    }
or
    include rclocal

Main usage is via a defined resource tye:

    rclocal::script { 'name':
      ensure   => 'present',
      content  => "content for rc_local snippet\n",
      priority => '44',
      autoexec => true,
    }

Or by defining hiera data for main class `scripts` parameter:

    ---
    rclocal::scripts:
      'name':
        'ensure': 'present'
        'content': "content for rc_local snippet\n"
        'priority': '44'
        'autoexec': true

and then consume the data in your profile:

    lookup('rclocal::scripts').each |$key, $params| {
      rclocal::script { $key:
        * => $params,
      }
    }

## Parameters

- ensure: set to 'present' or 'absent'
- content: the content you want to add
- priority: manages the order where the content appears
- autoexec: true or false - should puppet execute the code snippet from content - if set to false code will get executed on next reboot

## Development

If you encounter problems, please [open an issue](https://github.com/example42/puppet-rclocal/issues/new), if you miss functionality, we are happy to review [Pull Requests](https://github.com/example42/puppet-rclocal/pulls)

## Author

This module is maintained and created by [example42 GmbH](https://example42.com) (c) 2012-2018

