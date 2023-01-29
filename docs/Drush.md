# Drush

Drush is a command line tool that allows you to interact with Drupal. It's a very powerful tool that can be used to install Drupal, manage modules, themes, and much more.

## Instalation

It's pretty easy to install drush. On the terminal Go to your root drupal folder, and execute the following command:

```bash
composer require drush/drush:10.6.2
```

## Drush Commands i use most

```bash
# Cache clear
drush cc

# Cache and rebuild, same as domain.com/admin/rebuild.php
drush cr

# Export configuration, creates a configuration file in /config/sync
drush cex

# Import configuration, imports the configuration file from /config/sync
drush cim

# List modules/themes installed, can add flags to be more specific
# --status
# --type
drush pml

# Enable module
drush en MODULENAME

# Disable module
drush pmu MODULENAME

# Specific Migration commands

## drush migrate:import MIGRATIONNAME
drush mim MIGRATIONNAME

## drush migrate:rollback MIGRATIONNAME
drush mr MIGRATIONNAME

## drush migrate:reset MIGRATIONNAME
drush mrs MIGRATIONNAME
drush mrs MIGRATIONNAME --update
drush mrs MIGRATIONNAME --force

## drush migrate:status MIGRATIONNAME
drush ms MIGRATIONNAME

## drush migrate:messages MIGRATIONNAME
drush mmsg MIGRATIONNAME
```

