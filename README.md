## README - Drupal Scripts ##


These scripts are used on the Library's Drupal webheads for the purpose of moving code and content between the test, stage, and production environments.

Each of these scripts first determine what server they are running on using drush aliases. Based on this they perform the specified actions.

### What these scripts do ###

* drupal_backup : performs a backup of a drupal webhead environment
```
#!text

Creates a daily backup of the local Drupal instance's:
1. database content
2. static files
3. drupal document root
This script is run from druadmin's crontab

* drupal_copy_code : all webhead environments come in pairs - this script copies the Drupal code from webhead1 to webhead2 (or vice versa) in a given test, stage, or production realm
```
#!text

Order of operations:
1. Test ability to use drush to connect to each group of test, stage or production Drupal instances
2. Put the local (source) Drupal instance into ready-only maintenance mode
3. Rsync Drupal code files from the local Webhead to its fellow Webheads
4.  Apply any db changes, clear the Drupal cache, and turn maintenance mode off
```

* drupal_get_prod_content : copies the prodcution Drupal content (static files and databas) from prod to stage or prod to test
```
#!text

Order of operations:
1. Test ability to use drush to connect to the production Drupal instances
2. Copy the production static files to the directory used by local Drupal instance
3. Copy the production database to the database used by the local Drupal instance
4. Clear the local Drupal cache (in the database)
```

* drupal_promote_code : copies the Drupal code from the current environment to the next high environment (e.g. test to stage OR stage to prod)
```
#!text

Order of operations:
1. Test ability to use drush to connect to a destination site (stage or production)
2. Put the destination Drupal site into ready-only maintenance mode
3. Rsync Drupal code files from the SRC site to each DEST site
4. On the destination site, apply any db changes, clear the Drupal cache, and turn maintenance mode off
```

* drupal_sftp_content : copies the drupal content (static files and database) from the current environment to the Library's SFTP site for developers to download
```
#!text

Provide a daily copy of the local Drupal instance's content, suitable for development environments: MySQL database and static files
Send it to a subdirectory of our SFTP site www-sftp:/
This script is run from druadmin's crontab
```
