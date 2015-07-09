Redmine 2 kit for openshift
=========================

How to use
----------

1. Create a [openshift] (https://www.openshift.com/) account.
2. Add a new application with Ruby on Rails 4.
    * Ruby on Rails looks just a synonym for Ruby 2 + MySQL.
    * Set Source Code as `https://github.com/yutuo/openshift-redmine.git` `master`
    * Select `Ruby on Rails 4` and `MySQL 5.5` for cardridges.

3. Clone your repository from openshift

4. Overwrite redmine codes to the repository.
    * Overwrite config.ru.

5. Delelte followings folder.
    * /files
    * /log
    * /public/plugin_assets

6. If you want to use mails, configure config/configuration.yml.
    * You also need to remove a line with /config/configuration.yml from .gitignore.
    * Don't forget to remove `default:` settings.
    * As openshift does not provide a mail service, you should prepare a mail server outside openshift.
    * As openshift does not provide DNS service, you should specify `address` not with a host name but with a IP address.

8. Add all files, commit and push files.
    * It takes so much time to finish push.

9. Now the redmine is available. Login with username `admin` and password `admin`.

License
-------

openshift-redmine is unser GPL v2 as Redmine.
