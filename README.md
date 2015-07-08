Redmine kit for openshift
=========================

The OpenShift `ruby` cartridge documentation can be found at:

https://github.com/openshift/origin-server/tree/master/cartridges/openshift-origin-cartridge-ruby/README.md

Abstract
--------

This is a kit for deploying [Redmine] (http://www.redmine.org/) to [openshift] (https://www.openshift.com/).
Almost all the cases, you should use [openshift-redmine-quickstart] (https://github.com/openshift/openshift-redmine-quickstart).


Features
--------

* Version free.
    * [openshift-redmine-quickstart] (https://github.com/openshift/openshift-redmine-quickstart) is targeted for specific version,
        and not always available with the latest Redmine versions.
* Just only for Ruby 1.9.
    * [openshift-redmine-quickstart] (https://github.com/openshift/openshift-redmine-quickstart) is designed works both for Ruby 1.8 and Ruby 1.9.
        and it causes complicated codes.

How to use
----------

1. Create a [openshift] (https://www.openshift.com/) account.
2. Add a new application with Ruby on Rails.
    * Ruby on Rails looks just a synonym for Ruby 1.9 + MySQL.
    * Set Source Code as `https://github.com/ikedam/openshift-redmine.git` `master`
    * Select Ruby 1.9 and `MySQL 5.5` for cardridges.
3. Run following command:
    
        rhc env set REDMINE_LANG=ja -a Your_App_Name
    
4. Clone your repository from openshift
5. Overwrite redmine codes to the repository.
    * Overwrite config.ru.
6. Add followings to .gitignore if not exists.
    * /files/*
    * /log/*
    * /public/plugin_assets/*
7. If you want to use mails, configure config/configuration.yml.
    * You also need to remove a line with /config/configuration.yml from .gitignore.
    * Don't forget to remove `default:` settings.
    * As openshift does not provide a mail service, you should prepare a mail server outside openshift.
    * As openshift does not provide DNS service, you should specify `address` not with a host name but with a IP address.
8. Add all files, commit and push files.
    * It takes so much time to finish push.
9. Now the redmine is available. Login with username `admin` and password `admin`.

How does this works
-------------------

### What's required for Redmine to work on openshift

* Database parameters are provided via environment variables.
    * https://access.redhat.com/site/documentation/en-US/OpenShift_Online/2.0/html/User_Guide/Database_Environment_Variables4.html
    * Database name seems same to application name: https://www.openshift.com/blogs/mysql-55-database-hosting-is-now-on-openshift
* Data files, log files, and temporary files should be placed to the directories specified by environment variables.
    * https://access.redhat.com/site/documentation/en-US/OpenShift_Online/2.0/html/User_Guide/Directory_Environment_Variables4.html
    * https://access.redhat.com/site/documentation/en-US/OpenShift_Online/2.0/html/User_Guide/Logging_Environment_Variables.html
    * Following directories should be placed in those directories.
        * files
        * log
        * public/plugin_assets
* Run installation processes: http://www.redmine.org/projects/redmine/wiki/RedmineInstall
    1. install bundler
        * This is not needed as bundler is already available in openshift.
    2. install gems required by redmine using bundler
        * This is automatically executed if Gemfile is available.
        * But it requires Gemfile.lock is available, and it is not provided in redmine distributions.
    3. Create a key for session data.
    4. Create and update a db schema.
    5. Load the default data.
        * This should be run only once.

### What does this do

* database parameters are configured using eruby in config/database.yml
* Run commands using action_hooks. See http://openshift.github.io/documentation/oo_cartridge_developers_guide.html#openshift-builds for details.
    * .openshift/action/hooks/pre_build is executed when new commits are pushed and before ruby build phase is executed.
        * Install gems required by redmine using bundler
    * .openshift/action/hooks/deploy is executed when new commits are pushed.
        * Create and place directoreis.
        * Create a key for session data.
        * Create and update a db schema.
        * Load the default data. This is performed only when the database is empty.

License
-------

openshift-redmine is unser GPL v2 as Redmine.
