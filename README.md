# PHP (`php`)

**Part of the BAS Ansible Role Collection (BARC)**

Installs base PHP language

## Overview

* Installs PHP packages with `mcrypt` and `xdebug` modules and python tools for managing `.ini` files.
* Explicitly enables the `mcrypt` module (others are auto-enabled)
* Configures the `php.ini` file for the CLI interface.

## Author

[British Antarctic Survey](http://www.antarctica.ac.uk) - Web & Applications Team

Contact: [basweb@bas.ac.uk](mailto:basweb@bas.ac.uk).

## Availability

This role is designed for internal use but if useful can be shared publicly.

## License

[Open Government Licence V2](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/2/)

## Requirements

### BAS Ansible Role Collection (BARC)

* `core`

## Variables

### PHP interface `.ini` configuration (CLI, apache, etc.)

#### Overview

Each PHP *interface* (e.g. CLI, Apache, etc.) uses a `php.ini` to control default settings, logging, enabled modules, resource allocations, etc.

A python `.ini` plugin is used to parse and alter `php.ini` files for each interface, re-written files are stripped of comments but are otherwise the same as hand-edited versions. Using this approach allows any configuration option to be changed without having to explicitly convert each option into an Ansible variable, which would be tedious and hard to maintain.

An array of objects which represent options in the `.ini` file is used to selectively change options. Each object contains a reference to the `.ini` option and its updated value. The array of objects is an Ansible array and is configured like any other complex variable.

Due to a limitation with Ansible's syntax it is not possible to push or pop items from  array's. Therefore to override the default value of an Ansible object it would be necessary to replicate the entire Ansible array, including all other items you don't want to change, which, again, is tedious and hard to maintain (changing the default array directly is against best practice).

As a workaround two variables are used, a defaults array and a user array. The defaults array is run before the user array (which is by default empty). This means if you wanted to change a default value you would copy the array item from the default array into the user array with the override value.

#### All interfaces

Some `.ini` values should be changed in all implementations. To support this a *global* and *implementation specific* set of default and user variables are used.

* `php_all_ini_default_options`
	* These options are defaults for all PHP interfaces, this variable is effectively the highest level of option setting (applies to all interfaces and will be used by default).
    * **Do not override this variable**, use `php_all_ini_user_options` instead.
    * Structured as an array of items where each item consists of a section name string and array of options and values:
        * `section`  
            * Name of section in ini file (e.g. "Error handling and logging")
        * `options` [array]
            * `option`
                * Name of option in ini file (e.g. "display_errors") 
            * `value`
                * Value for option in ini file (e.g. "On") 
    * Default: (see variable)
* `php_all_ini_user_options`
    * Use to change values set in `php_all_ini_default_options` or where you want to change an option for all PHP interfaces.
    * Structured as an array of items where each item consists of a section name string and array of options and values:
        * `section`  
            * Name of section in ini file (e.g. "Error handling and logging")
        * `options` [array]
            * `option`
                * Name of option in ini file (e.g. "display_errors") 
            * `value`
                * Value for option in ini file (e.g. "On") 
    * Default: "[]  (empty array)" 

#### CLI interface `cli/php.ini`

See [here](http://php.net/manual/en/ini.php) for documentation.

* `php_cli_ini_default_options`
    * Default values for the CLI PHP implementation.
    * **Do not override this variable**, use `php_cli_ini_user_options` instead.
    * Structured as an array of items where each item consists of a section name string and array of options and values:
        * `section`  
            * Name of section in ini file (e.g. "Error handling and logging")
        * `options` [array]
            * `option`
                * Name of option in ini file (e.g. "display_errors") 
            * `value`
                * Value for option in ini file (e.g. "On") 
    * Default: (see variable)
* `php_cli_ini_user_options`
    * Use to change values set in `php_cli_ini_default_options` or where you want to change an option for the PHP CLI interface.
    * Structured as an array of items where each item consists of a section name string and array of options and values:
        * `section`  
            * Name of section in ini file (e.g. "Error handling and logging")
        * `options` [array]
            * `option`
                * Name of option in ini file (e.g. "display_errors") 
            * `value`
                * Value for option in ini file (e.g. "On") 
    * Default: "[]  (empty array)" 

## Changelog

### 0.2.3 - October 2014

* Adjusting role for inclusion in BARC

### 0.2.2 - September 2014

* Fixing `mcrypt` module loading

### 0.2.1 - August 2014

* Mail support has been extracted to relevant roles (currently only php-msmtp) since this isn't something unique to PHP (sending mail)

### 0.2.0 - August 2014

* Adopting new method of setting PHP ini config options using the ansible ini module
* This allows any config option to be set without first converting to an ansible variable
* Templating is now longer used as the ini file is modified directly
* Global config options (defined in this role) that apply to all implementations (CLI, apache, etc.) and per-implementation option variables are used to reuse defaults efficiently
* Specific variables are provided to override any default (either global or implementation specific) options where needed without having to also replicate other default variables not being overridden

### 0.1.1 - June 2014

* Fixing templating of PHP CLI file

### 0.1.0 - June 2014

* Initial version







