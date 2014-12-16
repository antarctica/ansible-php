# PHP (`php`)

**Part of the BAS Ansible Role Collection (BARC)**

Installs base PHP language

## Overview

* Installs PHP packages with `mcrypt` and `xdebug` modules and python tools for managing `.ini` files.
* Explicitly enables the `mcrypt` module (others are auto-enabled)
* Configures the `php.ini` file for the CLI interface.

## Availability

This role is designed for internal use but if useful can be shared publicly.

## Usage

### Requirements

#### BAS Ansible Role Collection (BARC)

* `core`

### Variables

#### PHP interface `.ini` configuration (CLI, apache, etc.)

##### Overview

Each PHP *interface* (e.g. CLI, Apache, etc.) uses a `php.ini` to control default settings, logging, enabled modules, resource allocations, etc.

A python `.ini` plugin is used to parse and alter `php.ini` files for each interface, re-written files are stripped of comments but are otherwise the same as hand-edited versions. Using this approach allows any configuration option to be changed without having to explicitly convert each option into an Ansible variable, which would be tedious and hard to maintain.

An array of objects which represent options in the `.ini` file is used to selectively change options. Each object contains a reference to the `.ini` option and its updated value. The array of objects is an Ansible array and is configured like any other complex variable.

Due to a limitation with Ansible's syntax it is not possible to push or pop items from  array's. Therefore to override the default value of an Ansible object it would be necessary to replicate the entire Ansible array, including all other items you don't want to change, which, again, is tedious and hard to maintain (changing the default array directly is against best practice).

As a workaround two variables are used, a defaults array and a user array. The defaults array is run before the user array (which is by default empty). This means if you wanted to change a default value you would copy the array item from the default array into the user array with the override value.

##### All interfaces

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

##### CLI interface `cli/php.ini`

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



## Developing

### Committing changes

The [Git flow](https://github.com/fzaninotto/Faker#formatters) workflow is used to manage development of this package.

Discrete changes should be made within *feature* branches, created from and merged back into *develop* (where small one-line changes may be made directly).

When ready to release a set of features/changes create a *release* branch from *develop*, update documentation as required and merge into *master* with a tagged, [semantic version](http://semver.org/) (e.g. `v1.2.3`).

After releases the *master* branch should be merged with *develop* to restart the process. High impact bugs can be addressed in *hotfix* branches, created from and merged into *master* directly (and then into *develop*).

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the BAS Web & Applications Team Jira project ([BASWEB](https://jira.ceh.ac.uk/browse/BASWEB)).


