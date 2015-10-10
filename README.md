# Capistrano::WPCLI

**Note: this plugin works only with Capistrano 3.**

Provides command line tools to facilitate Wordpress deploy.

## Installation

Add this line to your application's Gemfile:

    gem 'capistrano-wpcli'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install capistrano-wpcli

## Usage

All you need to do is put the following in `Capfile` file:

    require 'capistrano/wpcli'

### How it works

The following tasks are added to Capistrano:

* `wpcli:run`<br/>
Executes the WP-CLI command passed as parameter.<br/>
Example: `cap production wpcli:run["core language install fr_FR"]`
* `wpcli:db:push`<br/>
Pushes the local WP database to the remote server and replaces the urls.<br/>
Optionally backs up the remote database before pushing (if `wpcli_backup_db` is set to true, see Configuration).<br/>
Example: `cap production wpcli:db:push`
* `wpcli:db:pull`<br/>
Pulls the remote server WP database to local and replaces the urls.
* `wpcli:db:backup:remote`<br/>
Pulls the remote server WP database to localhost, uses `wpcli_local_db_backup_dir` to define the location of the export.
* `wpcli:db:backup:local`<br/>
Backs up the local WP database, uses `wpcli_local_db_backup_dir` to define the location of the export.
* `wpcli:rewrite:flush`<br/>
Flush rewrite rules.
* `wpcli:rewrite:hard_flush`<br/>
Perform a hard flush - update `.htaccess` rules as well as rewrite rules in database.
* `wpcli:uploads:rsync:push`<br/>
Push local uploads delta to remote machine using rsync.
* `wpcli:uploads:rsync:pull`<br/>
Pull remote uploads delta to local machine using rsync.

### Configuration

This plugin needs some configuration to work properly. You can put all your configs in Capistrano stage files i.e. `config/deploy/production.rb`.

Here's the list of options and the defaults for each option:

* `set :wpcli_remote_url`<br/>
Url of the WP root installation on the remote server (used by search-replace command).

* `set :wpcli_local_url`<br/>
Url of the WP root installation on the local server (used by search-replace command).

* `set :local_tmp_dir`<br/>
Absolute path to local directory temporary directory which is read and writeable. Defaults to `/tmp`.

* `set :wpcli_backup_db`<br/>
Set to true if you would like to create backups of databases on each push. Defaults to false.

* `set :wpcli_local_db_backup_dir`<br/>
Absolute or relative path to local directory for storing database backups which is read and writeable. Defaults to `config/backup`.<br/>
IMPORTANT: Make sure to add the folder to .gitignore to prevent db backups from being in version control.

* `set :wpcli_args`<br/>
You can pass arguments directly to WPCLI using this var. By default it will try to load values from `ENV['WPCLI_ARGS']`.

* `set :wpcli_local_uploads_dir`<br/>
Absolute or relative path to local WP uploads directory. Defaults to 'web/app/uploads/'.<br/>
IMPORTANT: Add trailing slash!

* `set :wpcli_remote_uploads_dir`<br/>
Absolute path to remote wordpress uploads directory. Defaults to "#{shared_path.to_s}/web/app/uploads/".<br/>
IMPORTANT: Add trailing slash!

* `set :wpcli_rsync_port`<br/>
If you for whatever reason need to set a different port for rsync you can do so by setting this var. Defaults to undefined.<br/>

### Vagrant

If you are using another machine as a development server (Vagrant for example), you should define a `dev` role and indicate the path where the project lives on that server. This normally goes on `deploy.rb` file. Here's an example:

`server "example.dev", user: 'vagrant', password: 'vagrant', roles: %w{dev}`

`set :dev_path, '/srv/www/example.dev/current'`

## Contributing

1. Fork it ( https://github.com/lavmeiker/capistrano-wpcli/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
